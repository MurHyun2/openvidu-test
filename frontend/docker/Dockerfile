FROM node:16-alpine3.16

# 기본 도구 설치 및 npm 설정
RUN apk add --no-cache git && \
    npm config set registry https://registry.npmjs.org/ && \
    npm config set fetch-retries 5 && \
    npm config set fetch-retry-mintimeout 20000 && \
    npm config set fetch-retry-maxtimeout 120000

# 작업 디렉토리 설정
WORKDIR /openvidu-library-react

# 패키지 파일만 먼저 복사하여 의존성 설치 (캐시 활용)
COPY package*.json ./

# 의존성 설치
RUN npm install --legacy-peer-deps

# 나머지 소스 복사
COPY . .

# 빌드 실행
RUN npm run build && \
    mkdir -p ./openvidu-basic-node/public && \
    cp -r ./build/* ./openvidu-basic-node/public/

# openvidu-basic-node 설정
RUN cp -r ./openvidu-basic-node /opt/openvidu-basic-node && \
    cd /opt/openvidu-basic-node && \
    npm install

WORKDIR /opt/openvidu-basic-node

COPY docker/entrypoint.sh .
RUN chmod +x entrypoint.sh

ENTRYPOINT ["./entrypoint.sh"]
