# Adapter contract

Only add a vendor adapter after documenting and checking its method against the exact supported models/BIOS versions. Files are installed as `/usr/lib/boot-logo/adapters/<vendor>.sh`.

Each adapter must define:

```bash
supports() { # manufacturer, product model, BIOS version
  # Return success only for exact, reviewed model/firmware combinations.
}

apply_logo() { # image path, manufacturer, product model, BIOS version
  # Return 0 only after the vendor method explicitly reports success.
  # Return nonzero on every unsupported, ambiguous, or failed result.
}
```

The live system stops with an error if the adapter is absent, the model is not allow-listed, or applying the image fails. Never infer support from the vendor name alone.
