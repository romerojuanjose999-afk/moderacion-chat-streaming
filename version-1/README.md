# Versión 1 · Productor–consumidor con archivos

Un productor simula el chat de un directo y escribe los mensajes como archivos; **Spark Structured
Streaming** los lee en micro-lotes, detecta groserías, spam y enlaces, y aplica strikes/baneos.
Una página web muestra el chat moderado y las métricas en vivo.

```
productor.py  →  archivos JSON  →  moderador_spark.py (Spark)  →  métricas  →  servidor_web.py (web)
```

## Requisitos

- Python 3.10+ y Java 17+ (para Spark)
- En Windows, los binarios de Hadoop (`hadoop/bin/winutils.exe`) ya vienen incluidos

## Ejecutar

```bash
pip install -r requirements.txt

# en 3 terminales
python moderador_spark.py
python servidor_web.py      # http://localhost:8000
python productor.py
```

En VS Code: `Ctrl+Shift+P` → **Tasks: Run Task** → **LiveMod: iniciar todo** abre las 3 terminales.
Con Docker: `docker compose up --build` (Spark UI en http://localhost:4040).

`python limpiar.py` deja todo en cero para una nueva demo.
