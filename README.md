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

### 🧩 7.1 Container কি?

ধরো তুমি একটা web app বানাচ্ছো — React frontend, Python API, আর PostgreSQL database। এখন সবার machine-এ same version এর Node, Python, PostgreSQL install থাকা, dependency conflict না হওয়া — এগুলো manage করা রীতিমতো ঝামেলার কাজ।

Container এই সমস্যার সমাধান। Container হলো তোমার app এর প্রতিটা component এর জন্য একটা isolated process — frontend, API, database প্রত্যেকটা নিজের মতো isolated environment এ চলে, একে অপরকে বা host machine কে touch করে না।

Container যেসব কারণে দারুণ:

বৈশিষ্ট্য	মানে কি
Self-contained	চলার জন্য যা লাগে সব ভেতরেই থাকে, host machine এ কিছু pre-install লাগে না
Isolated	একটা container অন্য container বা host কে প্রভাবিত করে না — security বাড়ে
Independent	একটা container delete করলে অন্যগুলোর কিছু হয় না
Portable	Laptop এ যেভাবে চলবে, data center বা cloud এও ঠিক একইভাবে চলবে

Container vs Virtual Machine (VM): VM হলো পুরো একটা OS — নিজের kernel, driver সহ। শুধু একটা app চালানোর জন্য পুরো VM তোলা অনেক বেশি overhead। Container শুধু একটা isolated process, সব container একই host kernel share করে — তাই অনেক lightweight, কম resource-এ বেশি container চালানো যায়। (তবে practice এ VM আর container একসাথেও ব্যবহার হয় — cloud এ যে machine পাও সেটা সাধারণত একটা VM, আর তার ভেতরে multiple container চলে।)

Basic commands:

```bash
docker run -d -p 8080:80 docker/welcome-to-docker   # container চালু করা
docker ps                                            # running container দেখা
docker ps -a                                         # stopped সহ সব container দেখা
docker stop <container-id>                           # container বন্ধ করা
```

### 🖼️ 7.2 Image কি?

Container যেহেতু একটা isolated process, তাহলে সে তার files আর configuration কোথা থেকে পায়? সেটাই হলো Image।

Image হলো একটা standardized package — এতে container চালানোর জন্য দরকারি সব file, binary, library, আর configuration থাকে। যেমন PostgreSQL image এ database binary + config file, আর একটা Python app এর image এ Python runtime + তোমার code + dependency সব থাকে।

দুইটা গুরুত্বপূর্ণ নিয়ম:

Image immutable — একবার বানানোর পর change করা যায় না, শুধু নতুন image বানাতে হয় বা তার উপর নতুন layer add করা যায়।
Image অনেকগুলো layer দিয়ে তৈরি — প্রতিটা layer একেকটা filesystem change represent করে (add/remove/modify)।

এই কারণে existing image কে base ধরে নিজের app বানানো যায় — যেমন Python image কে base ধরে শুধু তোমার app এর dependency আর code এর layer add করলেই হয়।

Image কোথায় পাবো? Docker Hub — default public registry, ১ লাখেরও বেশি ready image আছে। এখানে কিছু trusted category:

Docker Official Images — curated, সবচেয়ে secure
Docker Hardened Images — minimal, near-zero vulnerability, production এর জন্য
Docker Verified Publishers — verified কোম্পানির image
Docker-Sponsored Open Source — open-source project দের image

Basic commands:

```bash
docker search docker/welcome-to-docker   # image খোঁজা
docker pull docker/welcome-to-docker     # image download করা
docker image ls                          # local এ থাকা image গুলোর list
docker image history <image>             # image এর layer গুলো দেখা
```

### 📦 7.3 Registry কি?

Image local machine এ store করা যায়, কিন্তু অন্যদের সাথে share করতে বা অন্য machine এ ব্যবহার করতে হলে দরকার একটা centralized জায়গা — সেটাই Registry।

Registry হলো image store আর share করার centralized location — public বা private দুটোই হতে পারে। Docker Hub হলো default public registry। এছাড়া Amazon ECR, Azure ACR, Google GCR এর মতো অন্য registry-ও আছে, চাইলে নিজের private registry-ও (Harbor, JFrog Artifactory) চালানো যায়।

Registry vs Repository — পার্থক্য কি?

Registry = পুরো storage location (যেমন পুরো Docker Hub)
Repository = registry এর ভেতরে একটা folder এর মতো, related image গুলো একসাথে রাখা হয় (যেমন একটা project এর বিভিন্ন version/tag)

Registry
 ├── Repository A (project-a)
 │     ├── project-a:v1.0
 │     └── project-a:v2.0
 └── Repository B (project-b)
       ├── project-b:v1.0
       ├── project-b:v1.1
       └── project-b:v2.0

Basic workflow:

```bash
docker build -t <username>/my-app .          # Dockerfile থেকে image build
docker tag <username>/my-app <username>/my-app:1.0   # version tag দেওয়া
docker push <username>/my-app:1.0            # registry তে push করা
docker pull <username>/my-app:1.0            # registry থেকে pull করা
```

### 🧵 7.4 Docker Compose কি?

একটা container চালানো সহজ, কিন্তু বাস্তব app এ একসাথে অনেকগুলো service লাগে — database, cache, message queue ইত্যাদি। প্রতিটা docker run command আলাদা আলাদা চালিয়ে, network configure করে, cleanup করা — বেশ কষ্টের কাজ হয়ে যায়।

নিয়ম: একটা container একটা কাজই ভালোভাবে করবে — সব কিছু এক container এ গুঁজে দেওয়া উচিত না।

Docker Compose দিয়ে তোমার সব container আর তাদের configuration একটা single YAML file (compose.yaml) এ define করা যায়। এই file repo তে রাখলে, যে কেউ repo clone করে একটা মাত্র command দিয়েই পুরো app চালু করতে পারবে।

Compose একটা declarative tool — মানে তুমি শুধু define করে দাও কি চাও, বাকিটা Compose বুঝে নেয়। File এ change করলে আবার docker compose up দিলেই সেটা reconcile করে নেয়।

Dockerfile vs Compose file: Dockerfile দিয়ে image build হয়, Compose file দিয়ে running containers define হয়। প্রায়ই একটা Compose file একটা Dockerfile কে refer করে সেই service এর image বানানোর জন্য।

Basic commands:

```bash
docker compose up -d --build     # সব service (containers, network, volume সহ) চালু করা
docker compose down              # সব বন্ধ করে remove করা
docker compose down --volumes    # volume সহ পুরোপুরি clean করা
```

docker compose up চালালে একসাথে যা হয়:

দরকারি image গুলো Docker Hub থেকে pull হয়
App এর জন্য একটা network তৈরি হয়
Data persist করার জন্য volume তৈরি হয়
সব container একসাথে start হয়ে যায়
