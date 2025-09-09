# bunkr-albums.io Scraper (👉🏻 [Try Now](https://colab.research.google.com/github/profinderbro/bunker/blob/main/bunker.ipynb) Online)

### A Colab/Jupyter notebook for scraping and downloading videos from **bunkr-albums.io** links.  
---
```
!apt-get -qq install curl > /dev/null
# ⚠️👇🏻 Paste your links here, one per line (Which you get get in advance view for the bunker folder)
links_raw = """

Paste Here The URLs

"""
links = [line.strip() for line in links_raw.splitlines() if line.strip()]

import subprocess, re
mp4_links = []
for url in links:
    html = subprocess.check_output(["curl", "-sL", url], text=True)
    match = re.findall(r'https:\\/\\/i-kebab\.bunkr\.ru\\/thumbs\\/[^"]+?\.mp4', html)
    for m in match:
        clean = m.replace("\\/", "/")
        clean = clean.replace("https://i-kebab.bunkr.ru/thumbs/", "https://kebab.bunkr.ru/")
        mp4_links.append(clean)
print("\n".join(mp4_links))
```
