## 1. Docker আসলে কি?

Docker হলো একটা **open platform**, যেটা দিয়ে application develop, ship, আর run করা যায়।

সহজ কথায় — Docker তোমার **application কে infrastructure থেকে আলাদা করে দেয়**। মানে তুমি যে machine-এই deploy করো না কেন (নিজের laptop, কোনো server, বা cloud), application একই রকম ভাবে চলবে।

এই কাজটা Docker করে **container** নামের একটা lightweight, isolated environment এর মধ্যে application কে প্যাক করে। একটা container-এর ভেতরে application চলার জন্য যা যা দরকার (code, libraries, dependencies, config) — সব কিছু থাকে। তাই host machine-এ আলাদা করে কিছু install করা লাগে না।

---

## 2. কেন Docker ব্যবহার করবো?

### ✅ Fast & Consistent Delivery

Developer রা local এ code লিখে Docker container দিয়ে সবার সাথে share করতে পারে — সবাই exact same environment পায়। এতে CI/CD pipeline (test → fix → deploy) অনেক smooth হয়ে যায়।

### ✅ Easy Deployment & Scaling

Container গুলো অনেক **portable** — একই container laptop, data center, বা cloud (AWS, GCP, Azure) — যেকোনো জায়গায় চলবে। দরকার অনুযায়ী scale up/down করাও সহজ।

### ✅ Same Hardware-এ বেশি কাজ

Traditional Virtual Machine (VM) এর তুলনায় Docker container অনেক lightweight এবং fast। একই server-এ অনেকগুলো container চালানো যায় — resource কম লাগে, cost ও কমে।

---

## 3. Docker এর Architecture (কিভাবে কাজ করে)

Docker একটা **client-server architecture** ব্যবহার করে।

<img width="1233" height="651" alt="image" src="https://github.com/user-attachments/assets/3ed9386d-43b1-4dac-aff7-b2944f3b55a9" />


| Component | কাজ কি |
|---|---|
| **Docker Daemon (`dockerd`)** | মূল কাজের জায়গা — image build করা, container run করা, network/volume manage করা — সব `dockerd` করে |
| **Docker Client (`docker`)** | তুমি যখন `docker run` টাইপ command লিখো, সেটা client দিয়ে daemon এ পাঠানো হয় |
| **Docker Desktop** | Mac/Windows/Linux এর জন্য একটা easy-install app — এতে daemon, client, Compose, Kubernetes সব bundled থাকে |
| **Docker Registry** | Image store করার জায়গা। Default হলো **Docker Hub** (public registry), চাইলে নিজের private registry-ও রাখা যায় |

---

## 4. Docker এর মূল Objects

### 🖼️ Image
- একটা **read-only template**, যেখানে container বানানোর instruction লেখা থাকে।
- সাধারণত একটা image আরেকটা image এর উপর based হয় (যেমন: Ubuntu base + Apache + তোমার app)।
- Image বানাতে হয় **Dockerfile** দিয়ে। প্রতিটা instruction একটা করে **layer** তৈরি করে — Dockerfile change করলে শুধু changed layer গুলোই rebuild হয়, তাই এত fast আর lightweight।

### 📦 Container
- Image এর একটা **runnable instance**।
- Create, start, stop, move, delete — সবকিছু CLI বা API দিয়ে করা যায়।
- প্রতিটা container অন্য container ও host machine থেকে isolated থাকে (network, storage আলাদা করে control করা যায়)।
- Container remove করে ফেললে, যা persistent storage-এ save করা নেই তা হারিয়ে যায়।

**Example command:**
```bash
docker run -i -t ubuntu /bin/bash
```
এই command রান করলে কি হয়:
1. Local এ `ubuntu` image না থাকলে, registry (Docker Hub) থেকে pull করবে
2. নতুন একটা container create করবে
3. Container এর জন্য একটা read-write filesystem layer allocate করবে
4. Default network এ connect করার জন্য একটা network interface বানাবে (IP address সহ)
5. Container start করে `/bin/bash` run করবে
6. `exit` করলে container **stop** হবে, কিন্তু delete হবে না — চাইলে আবার start করা যাবে

---

## 5. Underlying Technology

- Docker লেখা হয়েছে **Go** language দিয়ে।
- Linux kernel এর **namespaces** feature ব্যবহার করে container এর জন্য isolated workspace তৈরি করে।
- প্রতিটা container এর জন্য আলাদা namespace set create হয়, যেটা isolation নিশ্চিত করে।

---

## 6. Quick Summary

| প্রশ্ন | উত্তর |
|---|---|
| Docker কি করে? | Application কে container-এ প্যাক করে, যেকোনো জায়গায় same ভাবে run করায় |
| কেন দরকার? | Consistent environment, fast CI/CD, easy scaling, hardware efficient |
| Main components | Client, Daemon, Registry, Images, Containers |
| কোথায় লেখা? | Go language, Linux namespaces এর উপর based |

---

### পরবর্তী ধাপ
- [Get Docker (install)](https://docs.docker.com/get-started/get-docker/)
- [Docker Concepts — Containers, Images, Registry, Compose](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)
