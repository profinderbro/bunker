# bunkr-albums.io Scraper  

A Colab/Jupyter notebook for scraping and downloading videos from **bunkr-albums.io** links.  
---
```
# Install dependencies
!apt-get -qq install curl wget aria2 > /dev/null

# ==== CONFIG ====
links_raw = """
https://bunkr.cr/f/2jscZ602iJjZX
https://bunkr.cr/f/f6wjYPbQR2mcJ
https://bunkr.cr/f/aVmPfP2ys16f6
https://bunkr.cr/f/I6uKyzh4lJoxP
https://bunkr.cr/f/5Sxyomg6YMUQv
https://bunkr.cr/f/BctVyAEtgrUSm
https://bunkr.cr/f/nBr3Z457IFkHc
https://bunkr.cr/f/wlwewNP8db5Qa
https://bunkr.cr/f/ncDzZ8nND8r92
https://bunkr.cr/f/CiAah2L2U12Ln
https://bunkr.cr/f/NWy9FadABPbtK
https://bunkr.cr/f/YvwbPo5IHB7Y5
https://bunkr.cr/f/uci2tuMrklr91
https://bunkr.cr/f/dFpPTKVbkNfOJ
https://bunkr.cr/f/1xIP1cMMEOjLV
https://bunkr.cr/f/RdRx1c3vt6UpR
https://bunkr.cr/f/9F5Y0lzQ8qdNV
"""

# Clean and split into list
links = [line.strip() for line in links_raw.splitlines() if line.strip()]

# ==== SCRAPE & DOWNLOAD ====
import subprocess, os, re

os.makedirs("/content/videos", exist_ok=True)
mp4_links = []

for url in links:
    html = subprocess.check_output(["curl", "-sL", url], text=True)
    match = re.findall(r'https[^"]+\.mp4', html)
    if match:
        mp4_links.append(match[0])

# Save all .mp4 links
with open("/content/urls.txt", "w") as f:
    f.write("\n".join(mp4_links))

# Download with aria2c
!aria2c -x 16 -s 16 -i /content/urls.txt -d /content/videos --continue

print("✅ Done. Videos in /content/videos")


```
