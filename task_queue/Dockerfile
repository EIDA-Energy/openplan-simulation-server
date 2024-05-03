FROM python:3.8-slim

ARG mvs_version

ENV MVS_VERSION $mvs_version

ENV CELERY_BROKER_URL redis://redis:6379/0
ENV CELERY_RESULT_BACKEND redis://redis:6379/0

RUN apt-get update && \
    apt-get install -y git && \
    apt-get install coinor-cbc -y && \
    apt-get install graphviz -y


COPY . /queue

# avoid running as root user
RUN useradd --create-home appuser
RUN chown -R appuser /queue
USER appuser

WORKDIR /queue

RUN ls -la

RUN python -m pip install --upgrade pip
RUN pip install -r requirements.txt --user

# Add the directory to PATH
ENV PATH="/home/appuser/.local/bin:${PATH}"
# Print the contents of the PATH for debugging
RUN echo $PATH
# Print the contents of the PYTHONPATH for debugging
RUN echo $PYTHONPATH

ENTRYPOINT celery -A tasks worker --loglevel=info --queues=$CELERY_TASK_NAME