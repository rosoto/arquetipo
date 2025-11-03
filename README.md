## 🚀 Creación del Arquetipo

Este script automatiza la creación de un nuevo proyecto Angular con la estructura de carpetas recomendada (basada en Features, Core, Shared y Layout).

### Instrucciones

1.  Copia el contenido del script de abajo en un archivo (ej: `crear_arquetipo.sh`).
2.  Otorga permisos de ejecución: `chmod +x crear_arquetipo.sh`
3.  Ejecútalo: `./crear_arquetipo.sh "nombre-de-tu-proyecto"`

Si no pasas un nombre, se te preguntará.

### Script (`crear_arquetipo.sh`)

```bash
#!/bin/bash

# --- Validación del Nombre del Proyecto ---
PROJECT_NAME=$1

if [ -z "$PROJECT_NAME" ]; then
  read -p "Por favor, ingresa el nombre del proyecto (ej: mi-app-angular): " PROJECT_NAME
fi

if [ -z "$PROJECT_NAME" ]; then
  echo "❌ Error: El nombre del proyecto no puede estar vacío."
  exit 1
fi

echo "------------------------------------------------"
echo "🚀 1/3: Creando nuevo proyecto Angular: $PROJECT_NAME"
echo "Configuración: Standalone, SCSS, No routing"
echo "------------------------------------------------"

# Crea el proyecto con configuraciones modernas:
# --standalone: Habilita la API standalone por defecto.
# --style=scss: Usa SCSS.
# --routing=false: Creamos nuestro propio app.routes.ts manualmente.
# --skip-install: Opcional, para instalar luego. Coméntalo si quieres que instale de inmediato.
ng new "$PROJECT_NAME" --standalone --style=scss --routing=false --skip-git --skip-install

# Si ng new falla, salimos del script
if [ $? -ne 0 ]; then
  echo "❌ Error: 'ng new' falló. Abortando."
  exit 1
fi

echo "------------------------------------------------"
echo "📂 2/3: Ingresando al directorio $PROJECT_NAME"
echo "------------------------------------------------"
cd "$PROJECT_NAME"

# Define la ruta base para los comandos 'ng generate'
APP_PATH="src/app"

echo "------------------------------------------------"
echo "🏗️ 3/3: Generando estructura de carpetas..."
echo "------------------------------------------------"

echo "Generando estructura CORE..."
ng g s $APP_PATH/core/services/auth --skip-tests
ng g interceptor $APP_PATH/core/interceptors/jwt --skip-tests
ng g guard $APP_PATH/core/guards/auth --skip-tests
mkdir $APP_PATH/core/models

echo "Generando estructura LAYOUT..."
ng g c $APP_PATH/layout/main-layout --standalone --skip-tests
ng g c $APP_PATH/layout/header --standalone --skip-tests
ng g c $APP_PATH/layout/footer --standalone --skip-tests

echo "Generando estructura SHARED..."
ng g c $APP_PATH/shared/components/button --standalone --skip-tests
ng g c $APP_PATH/shared/components/modal --standalone --skip-tests
ng g p $APP_PATH/shared/pipes/format-date --standalone --skip-tests
ng g d $APP_PATH/shared/directives/highlight --standalone --skip-tests
mkdir $APP_PATH/shared/models
mkdir $APP_PATH/shared/utils

echo "Generando estructura FEATURES (Ejemplo: Products)..."
ng g c $APP_PATH/features/products/components/product-list --standalone --skip-tests
ng g s $APP_PATH/features/products/services/products --skip-tests
mkdir $APP_PATH/features/products/models
# Creamos el archivo de rutas para la feature
touch $APP_PATH/features/products/products.routes.ts
echo "export const PRODUCTS_ROUTES = [];" > $APP_PATH/features/products/products.routes.ts

echo "Generando estructura ASSETS..."
mkdir -p src/assets/images src/assets/icons src/assets/fonts src/assets/i18n

echo "Limpiando archivos por defecto..."
# Eliminamos el contenido por defecto de app.component para dejar solo el router-outlet
echo "<router-outlet />" > $APP_PATH/app.component.html
# Eliminamos el CSS por defecto
> $APP_PATH/app.component.scss
# Creamos el archivo app.routes.ts
touch $APP_PATH/app.routes.ts
echo "import { Routes } from '@angular/router';

export const routes: Routes = [
  // { path: 'products', loadChildren: () => import('./features/products/products.routes').then(m => m.PRODUCTS_ROUTES) }
];
" > $APP_PATH/app.routes.ts

# Configuramos el app.config.ts para que use las rutas
# (Esto es un poco más avanzado para un script, pero es clave)
echo "import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';
import { provideHttpClient } from '@angular/common/http';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
    provideHttpClient() // Añadido para peticiones HTTP
  ]
};" > $APP_PATH/app.config.ts


echo "------------------------------------------------"
echo "✅ ¡Arquetipo '$PROJECT_NAME' creado exitosamente!"
echo "------------------------------------------------"
echo "Pasos siguientes:"
echo "1. cd $PROJECT_NAME"
echo "2. npm install (o yarn/pnpm)"
echo "3. ng serve -o"
```
