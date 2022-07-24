# Basic Recon Methodology
- [Subdomains Enumeration](#Subdomains-Enumeration)
## 1. Subdomains Enumeration

### 1. Active

- Amass subdomain enumeration using the command:

``` 
sudo docker run -v /home/mu57f4/bin/amass_output:/.config/amass/ amass enum -d domain.com
``` 

``` 
sudo docker run -v /home/mu57f4/bin/amass_output:/.config/amass/ amass enum -d domain.com
``` 

### 2. Passive

- Sublist3r using the command:

``` 
python3 [sublist3r.py](http://sublist3r.py) -d [domain.com](http://domain.com) -o domain-sublist3r-output.txt
``` 
- Merge the output of amass & sublist3r to get a collection of subdomains using this command:

``` 
cat amass-output.txt > sublist3r-output.txt ; cat sublist3r-output.txt | sort | uniq | tee domain-subs.txt
``` 

### 3. Live Subdomains

- Check the active subdomains with HTTPX using the command:

``` 
httpx -l path/to/domain-sublist3r-output.txt -ports "80|443|8000|8443” -o domain-active-subs.txt
``` 

- Now we have to files:
    1. domain-subs.txt
    2. domain-active-subs.txt

## 2. Subdomain Takeover

- Check the subdomains for takeover with tackover using the command:

```
python3 takeover.py -l domain-active-subs.txt -o domain-takeover.txt -t 5
``` 

## 3. Open Source Intelligence

- Check interesting subdomains for open source intelligence in Shodan & Censys

## 4. Waybackurls

- Grep extensions php, aspx, etc... and leaked files xlsx, docx, pdf, etc...
- Collect possible vulnerable links
- Extracting URLs from JS files searching for end-points, new subdomains in scope, APIs, Credentials, etc...

## 5. Nuclei Scanning

- Check for common known vulnerabilities in waybackurl links and domain-active-subs.txt file using this command:

``` 
nuclei -t nuclei-templates/ -l domain-active-subs.txt
``` 

## 6. Github

### 1. Automation

- Looking for Leaked data like credentials, tokens, secrets with Githound using the command:

``` 
echo "\"[domain.com](http://domain.com/)\"" | ./githound --dig-files --dig-commits --many-results --threads 5
``` 

### 2. Manually

- Using dorks to search manually in github

## 7. Content Discovery

### 1. Brute-force

- Brute force for directory, files, admin panels, Unauthorized access to some resource,  and parameters discovery searching for vulnerabilities using Tools:
    1. WFUZZ
    2. Dirsearch
    3. Gospider

## 8. Finding XSS in a wide scope

- After collecting parametric assets from waybackurls and spidered one use KXSS tool to find reflecting parameters and chars using the command:

``` 
cat urls | kxss > domain-kxss.txt
``` 
