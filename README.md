# Repositorio Padre con Submódulos (Git Submodule)

Este repositorio (`rpo_padre_submodules`) utiliza **Git Submodule** para incluir otros repositorios como subcarpetas. A diferencia de `git subtree`, cada submódulo es un repositorio Git independiente que el padre rastrea mediante un *puntero* a un *commit* SHA específico.

## 1. Estructura y Componentes

| Nombre del Proyecto | Rol | URL Remota | Carpeta Local | Remote Name (Default) |
| :--- | :--- | :--- | :--- | :--- |
| `rpo_padre_submodules` | **Padre** (Este Repo) | `https://github.com/scruc1990/rpo_padre_submodules.git` | N/A | `origin` |
| `rpo_hijo_1_submodules` | Hijo Estándar | `https://github.com/scruc1990/rpo_hijo_1_submodules.git` | `hijo_1/` | `origin` |
| `rpo_hijo_2_fork_submodules` | Hijo (Fork) | `https://github.com/Nest-ms-fh/rpo_hijo_2_fork_submodules.git` | `hijo_2/` | `origin` |

## 2. Clonación Inicial del Repositorio Completo

Cuando un colaborador clona este repositorio por primera vez, debe ejecutar dos pasos para obtener el código fuente de los submódulos:

1.  **Clonar el Repositorio Padre:**
    ```bash
    git clone [https://github.com/scruc1990/rpo_padre_submodules.git](https://github.com/scruc1990/rpo_padre_submodules.git)
    cd rpo_padre_submodules
    ```

2.  **Inicializar y Actualizar Submódulos:** Este comando descarga el contenido de los submódulos basándose en los *commits* que el padre está rastreando.
    ```bash
    git submodule update --init --recursive 
    ```

## 3. Flujo de Trabajo para Contribución (¡Crucial!)

La regla fundamental con submódulos es: **Los cambios en el submódulo se confirman primero en el hijo, y luego el puntero se actualiza en el padre.** 

### 3.1. Enviar Cambios a un Submódulo (Push)

Si deseas modificar el código de un hijo (ej. `hijo_1/`) y enviar esos cambios a su propio repositorio remoto:

1.  **Entrar al Submódulo y Trabajar:**
    ```bash
    cd hijo_1/
    # Hacer cambios, commit y push como un repo normal
    git add .
    git commit -m "Implementada nueva feature en hijo_1"
    git push origin main
    ```

2.  **Actualizar el Puntero del Padre:** Regresa al directorio raíz y confirma que el padre debe apuntar al nuevo *commit* del hijo. (Unicamente para la rama que este apuntando, es decir, master o main en su gran mayoria)
    ```bash
    cd .. # Regresar a la raíz del padre
    
    # El padre detecta que el puntero (SHA) de hijo_1 ha cambiado
    git add hijo_1 
    git commit -m "Actualizado el puntero del submódulo hijo_1"
    git push origin main
    ```
    *(Este último `push` actualiza el puntero para el resto de colaboradores).*

### 3.2. Traer Cambios de un Submódulo Remoto (Pull)

Si un colaborador hizo *push* de nuevos cambios a `rpo_hijo_2_fork_submodules.git`, debes hacer lo siguiente desde la **raíz del padre**:

1.  **Traer los nuevos punteros del padre:**
    ```bash
    git pull origin main
    ```
    *(Esto descarga el puntero actualizado del submódulo, pero **no el código**).*

2.  **Actualizar el Contenido de los Submódulos:** Descarga el código real de los hijos.
    ```bash
    git submodule update --remote 
    # O, si solo quieres actualizar uno:
    # git submodule update --remote hijo_2 
    ```

### 3.3. **Actualizar submodulos segun el puntero del padre**

Si deseas que los submódulos reviertan o avancen a la versión exacta que la rama del padre está rastreando:

```bash
git submodule update --recursive