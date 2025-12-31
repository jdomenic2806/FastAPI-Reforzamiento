# Reforzamiento de fastAPI

API REST simple para gestión de posts de blog construida con FastAPI.

## Requisitos

- Python 3.10+
- FastAPI
- Pydantic
- Uvicorn

## Instalación
```bash
# Clonar el repositorio
git clone <repo-url>
cd mini-blog

# Crear entorno virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate   # Windows

# Instalar dependencias
pip install "fastapi[standard]"
```

## Ejecución
```bash
# Modo desarrollo (con hot reload)
fastapi dev main.py

# Modo producción
fastapi run main.py

# Alternativa con uvicorn
uvicorn main:app --reload
```

La API estará disponible en `http://localhost:8000`

## Documentación Interactiva

- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

## Endpoints

| Método | Endpoint              | Descripción                |
|--------|-----------------------|----------------------------|
| GET    | `/`                   | Mensaje de bienvenida      |
| GET    | `/posts`              | Listar todos los posts     |
| GET    | `/posts?query=texto`  | Buscar posts por título    |
| GET    | `/posts/{post_id}`    | Obtener un post específico |
| POST   | `/posts`              | Crear nuevo post           |
| PUT    | `/posts/{post_id}`    | Actualizar un post         |
| DELETE | `/posts/{post_id}`    | Eliminar un post           |

## Ejemplos de Uso

### Listar posts
```bash
curl http://localhost:8000/posts
```

### Buscar posts
```bash
curl "http://localhost:8000/posts?query=fastapi"
```

### Obtener post (solo resumen)
```bash
curl "http://localhost:8000/posts/1?include_content=false"
```

### Crear post
```bash
curl -X POST http://localhost:8000/posts \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Nuevo post de prueba",
    "content": "Este es el contenido del post de prueba",
    "tags": [{"name": "python"}, {"name": "fastapi"}],
    "author": {"name": "Jose", "email": "jose@example.com"}
  }'
```

### Actualizar post
```bash
curl -X PUT http://localhost:8000/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"title": "Título actualizado", "content": "Contenido actualizado"}'
```

### Eliminar post
```bash
curl -X DELETE http://localhost:8000/posts/1
```

## Modelos de Datos

### PostCreate (entrada)
```json
{
  "title": "string (3-100 caracteres, sin 'spam')",
  "content": "string (mínimo 10 caracteres)",
  "tags": [{"name": "string"}],
  "author": {"name": "string", "email": "email"}
}
```

### PostPublic (respuesta)
```json
{
  "id": 1,
  "title": "string",
  "content": "string",
  "tags": [{"name": "string"}],
  "author": {"name": "string", "email": "email"}
}
```

## Validaciones

- **title**: 3-100 caracteres, no puede contener "spam"
- **content**: mínimo 10 caracteres
- **tags.name**: 2-30 caracteres
- **author.email**: formato email válido

## Estructura del Proyecto
```
mini-blog/
├── main.py          # Aplicación principal
├── README.md        # Este archivo
└── requirements.txt # Dependencias
```

## Notas

- Los datos se almacenan en memoria (se pierden al reiniciar)
- Para producción, integrar con base de datos (PostgreSQL, MongoDB, etc.)