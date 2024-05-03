FROM tiangolo/uvicorn-gunicorn-fastapi:python3.8

ARG mvs_version

ENV CELERY_BROKER_URL redis://redis:6379/0
ENV CELERY_RESULT_BACKEND redis://redis:6379/0

RUN apt-get update

COPY fastapi_app/ /fastapi_app

# avoid running as root user
RUN useradd --create-home appuser
RUN chown -R appuser /fastapi_app
USER appuser

WORKDIR /fastapi_app

RUN ls -la

RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt --user

# Add the directory to PATH
ENV PATH="/home/appuser/.local/bin:${PATH}"

# expose the app port
EXPOSE 5001
# Print the contents of the PATH for debugging
RUN echo $PATH
# Print the contents of the PYTHONPATH for debugging
RUN echo $PYTHONPATH
# run the app server, the last argument match the app variable in the webapp.py file
CMD ["uvicorn", "webapp:app", "--host", "0.0.0.0", "--port", "5001", "--proxy-headers"]
