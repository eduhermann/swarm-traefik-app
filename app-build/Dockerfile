FROM python:3.9-alpine

WORKDIR /app

RUN /usr/local/bin/python -m pip install --upgrade pip

RUN pip install click==8.0.3 Flask==2.0.2 itsdangerous==2.0.1 Jinja2==3.0.2 MarkupSafe==2.0.1 Werkzeug==2.0.2

COPY app.py .

EXPOSE 5000

CMD [ "flask", "run", "--host", "0.0.0.0" ]