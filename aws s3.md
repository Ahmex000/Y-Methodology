1. **Google Dork to Find $3 Buckets**
   - `site:s3.amazonaws.com site.com`
   - `site:amazonaws.com inurl:s3.amazonaws.com`
   - `site:s3.amazonaws.com intitle:index.of bucket`
   - `org:Target "S3_ACCESS_KEY_ID"`
   - `org:Target "S3_BUCKET"`
   - `org:Target "S3_ENDPOINT"`
   - `org:Target "S3_SECRET_ACCESS_KEY"`
   - `https://github.com/initstring/cloud_enum`
   - `site:http://s3.amazonaws.com "target.com"`

2. **Using Burp Suite**
   - Crawl the application through a browser proxy.
   - Use Burp Suite’s sitemap feature to discover $3 buckets.
   - Look for URLs or headers mentioning `s3.amazonaws.com` or `x-amz-bucket`.

3. **From Application**
   - Right-click on an image in the application and open it in a new tab.
   - Check if the URL format is `https://name.s3.amazonaws.com/image1.png`. The `name` before `.s3` is the bucket name.

4. **Online Tools on GitHub**
   - [S3Scanner](https://github.com/sa7mon/S3Scanner)
   - [Mass3](https://github.com/smiegles/mass3)
   - [slurp](https://github.com/3xbarath/slurp)
   - [lazyS3](https://github.com/nahamsec/lazys3)
   - [bucket_finder](https://github.com/msttwidmer/bucket_finder)
   - [ANSBucketDump](https://github.com/netgusto/ANSBucketDump)
   - [sandcastle](https://github.com/DxSearches/sandcastle)
   - [DumpsterDiver](https://github.com/securing/DumpsterDiver)
   - [$3 Bucket Finder](https://github.com/gwen001/s3-buckets-finder)
   - [S3Scanner](https://github.com/sa7mon/S3Scanner)
   - [Mass3](https://github.com/smiegles/mass3)
   - [slurp](https://github.com/3xbarath/slurp)
   - [lazyS3](https://github.com/nahamsec/lazys3)
   - [bucket_finder](https://github.com/msttwidmer/bucket_finder)
   - [ANSBucketDump](https://github.com/netgusto/ANSBucketDump)
   - [sandcastle](https://github.com/DxSearches/sandcastle)
   - [DumpsterDiver](https://github.com/securing/DumpsterDiver)
   - [$3 Bucket Finder](https://github.com/gwen001/s3-buckets-finder)
   - [grayhatwarfare](https://buckets.grayhatwarfare.com)
   - [osint.sh](https://osint.sh/buckets)
   - [s3-detect.yaml](https://github.com/projectdiscovery/nuclei-templates/blob/master/technologies/s3-detect.yaml)
   - [s3-detect.yaml](https://github.com/projectdiscovery/nuclei-templates/blob/master/technologies/s3-detect.yaml)

7. **Command to Extract $3 Buckets from JS URLs**
   ```bash
   cat js_url.txt | xargs -I {} curl -s {} | grep -oE 'http[s]?://[^/]*\.s3\.amazonaws\.com'
   ```

8. **Using Subfinder and Httpx**
   ```bash
   subfinder -d domain.com -all -silent | httpx -status-code -title -tech-detect | grep "Amazon S3"
   ```

9. **S3 Bucket Enumeration**
- S3Scanner:
  ```bash
  s3scanner -l domains.txt
  ```

---

- https://blog.vidocsecurity.com/blog/aws-s3-bucket-takeover/
- https://rhinosecuritylabs.com/penetration-testing/penetration-testing-aws-storage/
- https://labs.detectify.com/writeups/a-deep-dive-into-aws-s3-access-controls-taking-full-control-over-your-assets/
- https://cloudonaut.io/aws-security-primer/#Authorization
- https://github.com/initstring/cloud_enum
- https://www.intigriti.com/researchers/blog/hacking-tools/hacking-misconfigured-aws-s3-buckets-a-complete-guide
