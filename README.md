## 🚀 Generar Esqueleto de Carpetas

Si prefieres crear **únicamente la estructura de carpetas vacía** (sin archivos generados por la CLI), puedes usar este script.

```bash
#!/bin/bash
# Este script usa 'mkdir' para crear el esqueleto de directorios.

echo "Creando directorios en src/app..."

# 1. Core
mkdir -p src/app/core/guards
mkdir -p src/app/core/interceptors
mkdir -p src/app/core/services
mkdir -p src/app/core/models

# 2. Features (con ejemplos)
mkdir -p src/app/features/products/components
mkdir -p src/app/features/products/services
mkdir -p src/app/features/products/models
mkdir -p src/app/features/admin/components
mkdir -p src/app/features/admin/services

# 3. Shared
mkdir -p src/app/shared/components
mkdir -p src/app/shared/pipes
mkdir -p src/app/shared/directives
mkdir -p src/app/shared/models
mkdir -p src/app/shared/utils

# 4. Layout
mkdir -p src/app/layout/header
mkdir -p src/app/layout/footer
mkdir -p src/app/layout/sidebar
mkdir -p src/app/layout/main-layout

echo "Creando directorios en src/assets..."

# 5. Assets
mkdir -p src/assets/images
mkdir -p src/assets/icons
mkdir -p src/assets/fonts
mkdir -p src/assets/i18n

echo "✅ Esqueleto de carpetas creado."
```
