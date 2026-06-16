## Section 3 - General Skills & Web Recon

### Wave a Flag
- Downloaded a binary file using wget
- Made it executable: chmod +x warm
- Ran with help flag: ./warm -h
- Learned: Linux programs have -h or --help flags that reveal info

### Tab, Tab, Attack
- Used tab key for autocomplete in terminal
- Learned: Tab completion is essential for navigating long file paths fast

### Insp3ctOr
- Opened browser DevTools (F12) on a website
- Flag was split across 3 places: HTML comment, CSS file, JS file
- Learned: Always check page source, CSS, and JS files for hidden data

### strings it
- Used: strings <filename> | grep picoCTF
- Learned: strings command extracts readable text from binary files
- grep filters output to find specific patterns

### First Grep
- Used: grep picoCTF file
- Learned: grep searches for patterns inside files instantly

### where are the robots
- Visited /robots.txt on the website
- Found a Disallow entry pointing to a hidden page /cc6b1.html
- Visited that page to get the flag
- Learned: Always check robots.txt on web challenges — it reveals hidden pages

## Key Web Recon Checklist (learned today)
- /robots.txt
- /sitemap.xml
- Page source (Ctrl+U)
- Browser DevTools → Elements, Sources tabs
