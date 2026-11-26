# Versión 2 · Kafka + Spark + MySQL

Dos productores publican en **Kafka**, un único consumidor con **Spark Structured Streaming**
modera el chat y guarda cada micro-lote en **MySQL**.

```
productor_chat.py          ─► topic chat-bots ────────┐
productor_espectadores.py  ─► topic chat-espectadores ┴─► consumer.py (Spark) ─► MySQL (por lotes, JDBC)
   (Super Chats + espectadores reales desde la web)                         └─► web (métricas en vivo)
```

- Normaliza el texto (tildes, *leet speak*) y detecta groserías y spam con Spark SQL.
- `foreachBatch`: decide **aprobar / advertir / expulsar** y escribe el lote en MySQL.
- Conteos por ventanas de 10 s con `window` + `watermark`.
- Capa de base de datos orientada a objetos en `database/`.

## Servicios (Docker)

| Servicio | Puerto |
|----------|--------|
| Kafka 3.9 (KRaft, sin Zookeeper) | 9092 |
| MySQL 8.4 | 3307 (para no chocar con XAMPP) |
| Adminer | http://localhost:8080 |

## Ejecutar

```bash
docker compose up -d
pip install -r requirements.txt
python verificar.py               # revisa Java, Kafka, MySQL y puertos

# en terminales separadas
python consumer.py
python servidor_web.py            # http://localhost:8000
python productor_chat.py
python productor_espectadores.py
```

- `python ver_bd.py` — muestra en consola lo que Spark guardó en MySQL.
- `python reiniciar.py` — borra checkpoints, topics y tablas para una demo limpia.
- El video del directo es `web/video.mp4` (sirve cualquier `.mp4/.webm/.mov` de esa carpeta).
