# Amazon S3 – Simple Guide

## 📌 What is S3?

**Amazon S3 (Simple Storage Service)** is a cloud storage service by AWS.

It allows you to **store and retrieve files from the internet** anytime.

👉 Think of it like **Google Drive**, but for developers and applications.

---

## 📦 Key Concepts

### 1. Bucket

A **bucket** is like a folder where your files are stored.

* Each bucket has a **unique name**
* It exists in a specific **region**

---

### 2. Object

An **object** is a file stored in S3.

* Example: image.jpg, video.mp4, index.html
* Each object has:

  * Data (file)
  * Key (file name/path)
  * Metadata

---

### 3. Key

The **key** is the name/path of the file inside a bucket.

Example:

```
images/profile.png
```

---

## 🚀 Common Use Cases

* Store images, videos, documents
* Host static websites (HTML, CSS, JS)
* Backup and restore data
* Store logs and large datasets

---

## 🔐 Access & Permissions

By default:

* Buckets are **private**

You can:

* Make files **public**
* Control access using:

  * IAM policies
  * Bucket policies

---

## 🌐 Static Website Hosting

S3 can host static websites.

Steps:

1. Upload HTML, CSS, JS files
2. Enable "Static Website Hosting"
3. Make files public
4. Access via a public URL

---

## ⚡ Important Features

* **Highly durable** (data is safe)
* **Scalable** (store unlimited files)
* **Fast access**
* **Pay only for what you use**

---

## 💰 Pricing

You pay for:

* Storage (GB/month)
* Requests (GET, PUT)
* Data transfer

---

## 🧠 Simple Example

1. Create a bucket → `my-website`
2. Upload `index.html`
3. Enable static hosting
4. Open URL → Website live 🎉

---

## ⚠️ Things to Remember

* Bucket names must be **globally unique**
* Don’t expose sensitive data publicly
* Use proper permissions

---

## 🏁 Conclusion

Amazon S3 is a simple and powerful way to store and serve files on the cloud.
It is widely used for **static websites, backups, and file storage**.

---

👉 That’s all you need to get started with S3 🚀
