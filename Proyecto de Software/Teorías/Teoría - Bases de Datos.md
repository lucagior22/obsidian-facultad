
## SQL
*Structured Query Language*

SQl = Lenguaje 
PostgreSQL, MySQL = Motores de BD

## PostgreSQL

**PGAdmin, DBeaver**: Herramientas para gestionar la instancia de la base de datos.

Accedemos con el módulo/driver *psycopg2*.

```python
import psycopg2

# Conectar a la BD
conn = psycopg2.connect(
					   host="localhost",
					   database="proyecto_db",
					   user="proyecto_db",
					   password="proyecto_db")

cur = conn.cursor()

# Ejecutar una consulta
cur.execute("select * from issues")
conn.commit()

issues = cur.fetchall()

cur.close()
conn.close()
```
*No se reciben los issues como diccionario*

Para que se devuelvan los datos formateados correctamente (como *dict*):
```python
import psycopg2, psycopg2.extras

#...
cur = conn.cursor(cursor_factory=psycopg2.extras.RealDictCursor)
#...
```
