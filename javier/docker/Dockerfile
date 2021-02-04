FROM python:3.9-alpine
COPY flask .
RUN pip install --no-cache-dir -r requirements.txt
RUN python db.py
ENTRYPOINT ["python", "runserver.py"]