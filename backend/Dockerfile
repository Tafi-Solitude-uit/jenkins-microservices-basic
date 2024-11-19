FROM node:14-alpine3.12

# Đặt thư mục làm việc
WORKDIR /app

# Sao chép file package.json và package-lock.json trước
COPY package*.json ./

# Cài đặt dependencies
RUN npm install

# Sao chép toàn bộ nội dung vào container
COPY . .

EXPOSE 4000

CMD ["npm","start"]
