FROM python:3.9.18-slim-bookworm


WORKDIR /python-docker

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
COPY . .
# Make the bundled VADER lexicon available to NLTK
RUN mkdir -p /usr/share/nltk_data/sentiment && \
    cp sentiment/vader_lexicon.zip /usr/share/nltk_data/sentiment/
RUN ls
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0"]
