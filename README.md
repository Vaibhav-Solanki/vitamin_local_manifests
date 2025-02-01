# vitamin_local_manifests
 
📌 **Building Rome**  

To build **Rome**, follow these steps:  

1️⃣ **Locate `vitamin.xml`**  
📍 Repository: [Vitamin Local Manifests](https://github.com/Vaibhav-Solanki/vitamin_local_manifests)  

2️⃣ **Place `device.xml`**  
Move it to:  
📂 `.repo/local_manifests/device.xml`  

3️⃣ **Sync the Repo**  
Run the following command:  
```bash
repo sync -c -j$(nproc --all)
```

✅ Once synced, proceed with the build process! 🚀
