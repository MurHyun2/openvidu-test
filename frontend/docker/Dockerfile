FROM node:16-alpine3.16

# 필수 시스템 도구 설치
RUN apk add --no-cache python3 make g++

# npm 9+로 업그레이드 (Node 16 호환)
RUN npm install -g npm@9

WORKDIR /openvidu-library-react

# 패키지 파일 분리 복사 (캐싱 최적화)
COPY package*.json ./

# 강제 설치 및 경고 억제
RUN npm config set fund false && \
    npm install --legacy-peer-deps --force --omit=optional

# 소스 코드 복사
COPY . .

# 프로덕션 빌드
RUN npm run build && \
    cp -r ./build ./openvidu-basic-node/public

# 후속 단계 유지
RUN cp -r ./openvidu-basic-node /opt/openvidu-basic-node && \
    npm --prefix /opt/openvidu-basic-node install

WORKDIR /opt/openvidu-basic-node
COPY docker/entrypoint.sh .
ENTRYPOINT ["./entrypoint.sh"]
