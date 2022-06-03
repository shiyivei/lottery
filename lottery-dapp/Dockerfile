FROM node:16-alpine

RUN mkdir -p /usr/app/
WORKDIR /usr/app/

COPY ./ ./

RUN npm install
RUN npm run build

# 暴露端口号
EXPOSE 3000

CMD ["npm","start"]