FROM node:19.9-buster-slim

# コンテナ内の作業ディレクトリを作成
WORKDIR /app
# パッケージ一覧(インデックス)を更新
COPY docker-vite/ ./
RUN apt update \
    # && yarn install
    && yarn    
#  \
# &&  mv ./node_modules /

# ポート番号3000でコンテナを公開
EXPOSE 3000