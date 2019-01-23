FROM node:4.2
COPY . /src
RUN cd /src && npm install
EXPOSE 9393
CMD ["node", "/src/server.js"]
