# Docker Desktop: Bind Mount ও Live Reload

Docker container-এর ভেতরে code পরিবর্তন করলে সাথে সাথে যেন সেটা দেখা যায়, তার জন্য `nodemon` এবং `bind mount` ব্যবহার করা হয়। নিচে ধাপে ধাপে পুরো প্রক্রিয়াটি দেখানো হলো।

---

## ১. `nodemon` Install করা

`nodemon` package install করলে বার বার terminal-এ গিয়ে project run করে update দেখার প্রয়োজন হয় না। কোনো কিছু পরিবর্তন করলেই সেটা নিজে থেকে project restart করে দেখিয়ে দেয়।

```bash
npm i nodemon
```

---

## ২. `package.json`-এ Script যোগ করা

```json
"dev": "nodemon index.js"
```

সম্পূর্ণ `package.json` ফাইলটি এমন দেখাবে:

```json
{
  "name": "node-app",
  "version": "1.0.0",
  "description": "Docker Course",
  "license": "ISC",
  "author": "",
  "type": "commonjs",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "express": "^5.2.1",
    "nodemon": "^3.1.14"
  }
}
```

---

## ৩. Terminal-এ Command চালানো

```bash
npm run dev
```

এখন কোনো কিছু update করলে browser-এ automatically সেই update দেখা যাবে।

---

## ৪. Globally Watch করার জন্য (Docker Container-এর ভেতরে)

Docker container-এর ভেতরে file system watch ঠিকমতো কাজ না করলে `--legacy-watch` flag ব্যবহার করতে হয়:

```json
"dev": "nodemon --legacy-watch index.js"
```

সম্পূর্ণ `package.json`:

```json
{
  "name": "node-app",
  "version": "1.0.0",
  "description": "Docker Course",
  "license": "ISC",
  "author": "",
  "type": "commonjs",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon --legacy-watch index.js"
  },
  "dependencies": {
    "express": "^5.2.1",
    "nodemon": "^3.1.14"
  }
}
```

---

## ৫. Dockerfile Update করা

- `RUN npm install -g nodemon` — এই line দিয়ে `nodemon` globally install হবে
- `WORKDIR /app` — এই line দিয়ে work directory নির্ধারণ করা হবে
- `CMD ["npm", "run", "dev"]` — এই line container চালু হওয়ার সময় যে command চলবে সেটা bole দেয়

```dockerfile
FROM node:latest
RUN npm install -g nodemon
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "run", "dev"]
```

---

## ৬. Docker Image তৈরি করা

```bash
docker build -t my-node-app .
```

---

## ৭. Volume সহ Container তৈরি করা

এই command চালালে container তৈরি হবে, terminal-এ project run হবে, এবং code-এ কোনো পরিবর্তন করলে browser-এর output সাথে সাথে update হয়ে যাবে (bind mount-এর কারণে)।

```bash
docker run --name my-containers -p 3000:3000 --rm -v "C:/Users/User/Desktop/Node App:/app" my-node-app
```

> এখানে `-v "C:/Users/User/Desktop/Node App:/app"` অংশটি হলো bind mount — local machine-এর folder-কে সরাসরি container-এর `/app` folder-এর সাথে connect করে দেয়, ফলে দুই জায়গার file সবসময় sync থাকে।

---
