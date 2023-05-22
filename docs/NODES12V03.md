# VIDEO 03 - Migración de modelos, routers y middlewares

En este vídeo hemos tenido que dedicar más tiempo, ya que hemos migrado todos los modelos, controladores de rutas y middlewares de nuestra aplicación.

Para no extendernos indicaremos solo los relacionados con Car:

Modelo Car.ts

```tsx
import mongoose, { type ObjectId } from "mongoose";
const Schema = mongoose.Schema;

interface ICar {
  brand: ObjectId;
  model: string;
  plate: string;
  power: number;
  owner: ObjectId;
}

// Creamos el schema del coche
const carSchema = new Schema<ICar>(
  {
    brand: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "Brand",
      required: false,
    },
    model: {
      type: String,
      required: true,
      trim: true,
      minLength: 3,
      maxLength: 40,
    },
    plate: {
      type: String,
      required: false,
      trim: true,
      minLength: 3,
      maxLength: 20,
    },
    power: {
      type: Number,
      required: false,
      min: 5,
      max: 2000,
    },
    owner: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "User",
      required: false,
    },
  },
  {
    timestamps: true,
  }
);

export const Car = mongoose.model<ICar>("Car", carSchema);
```

Rutas car.routes.ts:

```tsx
import express from "express";
import fs from "fs";
import multer from "multer";

// Modelos
import { Brand } from "../models/Brand";
const upload = multer({ dest: "public" });

export const brandRouter = express.Router();

// CRUD: READ
brandRouter.get("/", async (req, res, next) => {
  try {
    // Asi leemos query params
    const page = req.query.page ? parseInt(req.query.page as string) : 1;
    const limit = req.query.limit ? parseInt(req.query.limit as string) : 10;
    const brands = await Brand.find()
      .limit(limit)
      .skip((page - 1) * limit);

    // Num total de elementos
    const totalElements = await Brand.countDocuments();

    const response = {
      totalItems: totalElements,
      totalPages: Math.ceil(totalElements / limit),
      currentPage: page,
      data: brands,
    };

    res.json(response);
  } catch (error) {
    next(error);
  }
});

// CRUD: READ
brandRouter.get("/:id", async (req, res, next) => {
  try {
    const id = req.params.id;
    const brand = await Brand.findById(id);
    if (brand) {
      res.json(brand);
    } else {
      res.status(404).json({});
    }
  } catch (error) {
    next(error);
  }
});

// CRUD: Operación custom, no es CRUD
brandRouter.get("/name/:name", async (req, res, next) => {
  const brandName = req.params.name;

  try {
    const brand = await Brand.find({ name: new RegExp("^" + brandName.toLowerCase(), "i") });
    if (brand?.length) {
      res.json(brand);
    } else {
      res.status(404).json([]);
    }
  } catch (error) {
    next(error);
  }
});

// CRUD: CREATE
brandRouter.post("/", async (req, res, next) => {
  try {
    const brand = new Brand(req.body);
    const createdBrand = await brand.save();
    return res.status(201).json(createdBrand);
  } catch (error) {
    next(error);
  }
});

// CRUD: DELETE
brandRouter.delete("/:id", async (req, res, next) => {
  try {
    const id = req.params.id;
    const brandDeleted = await Brand.findByIdAndDelete(id);
    if (brandDeleted) {
      res.json(brandDeleted);
    } else {
      res.status(404).json({});
    }
  } catch (error) {
    next(error);
  }
});

// CRUD: UPDATE
brandRouter.put("/:id", async (req, res, next) => {
  try {
    const id = req.params.id;
    const brandUpdated = await Brand.findByIdAndUpdate(id, req.body, { new: true, runValidators: true });
    if (brandUpdated) {
      res.json(brandUpdated);
    } else {
      res.status(404).json({});
    }
  } catch (error) {
    next(error);
  }
});

brandRouter.post("/logo-upload", upload.single("logo"), async (req, res, next) => {
  try {
    // Renombrado de la imagen
    const originalname = req.file?.originalname as string;
    const path = req.file?.path as string;
    const newPath = `${path}_${originalname}`;
    fs.renameSync(path, newPath);

    // Busqueda de la marca
    const brandId = req.body.brandId;
    const brand = await Brand.findById(brandId);

    if (brand) {
      brand.logoImage = newPath;
      await brand.save();
      res.json(brand);

      console.log("Marca modificada correctamente!");
    } else {
      fs.unlinkSync(newPath);
      res.status(404).send("Marca no encontrada");
    }
  } catch (error) {
    next(error);
  }
});
```
