# Docker Bind Mount

## Bind Mount কি?

Bind Mount হলো Docker-এর এমন একটি ব্যবস্থা, যেখানে Host Machine-এর একটি নির্দিষ্ট Folder বা File সরাসরি Container-এর একটি Folder-এর সাথে যুক্ত (mount) করা হয়।

সহজভাবে বললে:
> Host-এর Folder ↔ Container-এর Folder

তুমি Host-এর ফাইলে যা পরিবর্তন করবে, Container সেই পরিবর্তন সঙ্গে সঙ্গে দেখতে পাবে। আবার Container যদি ওই Folder-এ কিছু লিখে, সেটাও Host-এর Folder-এ দেখা যাবে।

## কিভাবে কাজ করে

```bash
docker run -v /host/path:/container/path my-image
```

অথবা newer syntax:

```bash
docker run --mount type=bind,source=/host/path,target=/container/path my-image
```

Host-এর `/host/path` এর কোনো file change করলে সেটা সাথে সাথে container-এর `/container/path`-এও reflect হবে, কারণ underlying data actual একই — শুধু দুই জায়গা থেকে access হচ্ছে।

## Bind Mount vs Volume

| বিষয় | Bind Mount | Volume |
|---|---|---|
| Location | Host-এর যেকোনো path | Docker managed area (`/var/lib/docker/volumes`) |
| Control | User পুরো path control করে | Docker control করে |
| Portability | কম (host path-এর উপর depend করে) | বেশি |
| Use case | Development, local file access | Production, data persistence |

## কখন ব্যবহার হয়

- **Development-এর সময়**: source code host-এ রেখে container-এ live reflect করানোর জন্য (যেমন hot-reload)
- Host-এর কোনো specific config file বা log directory container-এ share করতে হলে

## গুরুত্বপূর্ণ পয়েন্ট

- Bind mount ব্যবহার করলে container, host machine-এর file structure-এর উপর dependent হয়ে যায় — তাই এটা highly portable না।
- Security concern আছে, কারণ container host-এর sensitive path access করতে পারে যদি ভুলভাবে mount করা হয়।
- Path না থাকলে Docker automatically সেই directory host-এ create করে ফেলতে পারে (এটা মাঝে মাঝে unexpected বাগ তৈরি করে)।
