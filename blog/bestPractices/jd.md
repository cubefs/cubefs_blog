---
blog: true
---

## Challenge
JD.com’s Infrastructure Department is responsible for developing the hyperscale containerized and Kubernetes platform that powers all facets of JD’s business, including the website, which serves over 380 million active customers. In 2016, the team was in need of a cloud native registry to provide maintenance for its private image central repository.

## Solution
After considering a number of different solutions, including using the native registry, JD decided to go with Harbor. “We found that Harbor is most suitable in the Kubernetes environment, and it can help us address our challenges,” says Vivian Zhang, JD.com Product Manager and CNCF Ambassador. “We have been a loyal user of Harbor since the very beginning.”

## Impact
“We save approximately 60% of maintenance time for our private image central repository due to the simplicity and stability of Harbor,” says Zhang. Plus, Harbor has enabled authorization authentication and access control for images, which hadn’t been possible before. 

![pic](/images/blog/jd01.png)

JD.com is the world’s third largest internet company by revenue, and at its heart it considers itself “a science and technology company with retail at its core,” says Vivian Zhang, Product Manager at JD.com and CNCF Ambassador.

JD’s Infrastructure Department is responsible for developing the hyperscale containerized and Kubernetes platform that powers all facets of JD’s business, including the website, which serves more than 380 million active customers, and its e-commerce logistics infrastructure, which delivers over 90% of orders same- or next-day.

In 2016, the team was in need of a cloud native registry to provide maintenance for its private image central repository. Specifically, the platform needed a solution for:

1. User authorization and basic authentication
1. Access control for authorized users
1. UI management for the JD Image Center
1. Image vulnerability scanning for image security

![pic](/images/blog/jd02.png)

The team evaluated a number of different solutions, including the native registry. But there were two main problems with the native registry: the need to implement authorization certification, and having the meta information on images traverse the file system. As such, with the native registry, “the performance could not meet our requirements,” says Zhang. 

Harbor, a cloud native registry project that just graduated within CNCF, turned out to be “the best fit for us,” says Zhang. “Harbor introduces the database, which can accelerate the acquisition of image meta information. Most importantly, it delivers better performance.”

Harbor also integrates Helm’s chart repository function to support the storage and management of the Helm chart, and provides user resource management, such as CPU, memory, and disk space — which were valuable to JD. 

“We found that Harbor is most suitable in the Kubernetes environment, and it can help us address our challenges,” says Zhang. “We have been a loyal user of Harbor since the very beginning.”

![pic](/images/blog/jd03.png)

 In order for it to work best at JD, “Harbor is used in concert with other products to ensure maximum efficiency and performance of our systems,” says Zhang. Those products include the company’s own open source project (and now a CNCF sandbox project), ChubaoFS, a highly available distributed file system for cloud native applications that provides scalability, resilient storage capacity, and high performance. With ChubaoFS serving as the backend storage for Harbor, multiple instances in a Harbor cluster can share container images. “Because we use ChubaoFS, we can achieve a highly available image center cluster,” says Zhang.
 
 ![pic](/images/blog/jd04.png)
 
 Indeed, Harbor has made an impact at JD. “We save approximately 60% of maintenance time for our private image central repository due to the simplicity and stability of Harbor,” says Zhang. Plus, Harbor has enabled authorization authentication and access control for images, which hadn’t been possible before.

JD’s hyperscale — several thousand nodes, tens of thousands of container images, and dozens of terabytes of container image data volume — has made some specific best practices necessary. “Our several data centers are located in different areas, so there can be performance challenges when pulling images,” says Zhang. “In order to resolve this problem, we build Harbor clusters in local data centers with caches, then optimize the performance of image pulls and enhance the quality of service.”

![pic](/images/blog/jd05.png)
 
![pic](/images/blog/jd06.png)
  
All in all, “Harbor is simple and easy to use,” says Zhang. “We recommend Harbor for its stability, computing resource management, UI management, integrated chart, and image vulnerability scanning features.”

Just as importantly, Harbor is a good fit for JD’s Retail-as-a-Service strategy. “Harbor is an open source project, which enables it to keep improving with the help of a large number of contributors,” says Zhang. “This aligns with our broader mission to open our technology to others in order to enable partners in a wide range of industries to benefit.”