---
title: " Humane World for Animals uses AWS to scale global animal welfare programs"
weight: 3.2
pre: " <b> 3.2. </b> "

---

![Blog Header](/images/blog/blog2/Blog-Header.png)
*Humane World for Animals AWS Implementation*

### **Humane World for Animals uses AWS to scale global animal welfare programs**

by Stacy Stonich and Jules Marenghi | on 24 SEP 2025 | in [Customer Solutions](https://aws.amazon.com/blogs/publicsector/category/post-types/customer-solutions/), [Nonprofit](https://aws.amazon.com/blogs/publicsector/category/public-sector/nonprofit/), [Public Sector](https://aws.amazon.com/blogs/publicsector/category/public-sector/)  | [Permalink](https://aws.amazon.com/blogs/publicsector/humane-world-for-animals-uses-aws-to-scale-global-animal-welfare-programs/) 

For over 70 years, [Humane World for Animals](https://www.humaneworld.org/en)—formerly the Humane Society of the United States and Humane Society International—has been a pioneer in the animal protection movement, addressing the root causes of animal cruelty to drive lasting change and create a better world for all animals globally. The organization has helped build a more humane world for animals, helping to pass thousands of groundbreaking laws, rescuing hundreds of thousands of individual animals, and caring for and protecting millions more.

To address the global challenge of managing street animal populations, Humane World for Animals has developed Humane World Apps—a comprehensive global data management tool designed to improve dog and cat welfare programs through spay and neuter management, mass vaccination, and access to care. With support from the [AWS Imagine Grant](https://aws.amazon.com/government-education/nonprofits/aws-imagine-grant-program/)—a public grant program that provides both cash and [Amazon Web Services (AWS)](https://aws.amazon.com/) credits to registered nonprofits using cloud technology to advance their mission—Humane World Apps is transforming how large-scale animal welfare programs operate worldwide.

During the beta phase in Vadodara, India, Humane World Apps achieved a dog sterilization rate of up to 86% and a 60% reduction in dog-related complaints after app deployment. This technology aims to end the suffering of companion animals by improving program efficiency, supporting community health, and promoting innovative sustainable solutions to reduce human-dog conflict.

In this article, we will present how Humane World Apps is revolutionizing animal welfare management through cloud technology and data-driven solutions.

### **Fragmented data collection in global animal welfare**

The scale of the challenge in managing street animal populations is enormous. It is estimated that there are between 700 million and 1 billion dogs worldwide, including both owned and unowned dogs living in shelters, families, or freely in urban and rural environments. According to a [MARS study](https://www.mars.com/news-and-stories/press-releases-statements/pet-homelessness), there are nearly 362 million stray dogs and cats in 20 countries. Communities and government agencies—especially in developing economies—struggle to find reasonable, humane, and accessible solutions to manage dog and cat populations, promote animal welfare, and build peaceful coexistence between companion animals and humans.

Governments are responsible for community health and safety, but many places still lack specific solutions and evidence-based programs. Additionally, many places still apply lethal methods to street animals. Long-term solutions including humane and effective sterilization, vaccination, and access to affordable veterinary services are key to improving animal welfare and reducing human-companion animal conflict in areas with stray animal problems.

History shows that large-scale dog and cat welfare programs often struggle due to ineffective, fragmented data management and collection. The consequence is suboptimal results in controlling stray dog and cat populations and addressing community health issues.

### **Building a comprehensive solution on AWS**

The goal of Humane World Apps is to provide a unified, high-tech platform that integrates data management systems for activities such as spay and neuter, mass vaccination, and veterinary clinic operations. The application includes both mobile applications and web interfaces, providing advanced features tailored to each program's needs.

The mobile application aims to support field teams in collecting real-time data and recording capture locations to ensure dogs are returned to their original locations. The system is expected to track the entire care cycle, including pre- and post-surgical care, surgical data, and image identification to monitor individual animals as well as program outcomes. Users can expect the application to manage rabies vaccination programs through geo-fencing, GPS positioning, and automatic reporting features.

Since Humane World for Animals is a nonprofit organization, the initial beta version of the application was built on a limited budget. Some developers were volunteers, and they designed and built the application architecture using free or low-cost services. With support from the AWS Imagine Grant (Amazon Web Services' grant program for nonprofits), the organization is now in the process of transitioning the beta application to AWS services to unify and simplify the technology system.

Currently, the application is designed to use a single [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) (Amazon EC2) server to host both production and testing environments. Additionally, database servers for production and testing environments use [Amazon Relational Database Service](https://aws.amazon.com/rds/) (Amazon RDS). The application uses [Amazon Simple Storage Service](https://aws.amazon.com/s3/) (Amazon S3) to store animal images. During the transition, the organization has begun using [Amazon Simple Email Service](https://aws.amazon.com/ses/) (Amazon SES), and plans to leverage additional AWS tools in the future.

To ensure redundancy, the organization maintains an AWS instance in Ohio for cold standby. The production and testing environments in Northern Virginia currently use Cloudflare CDN, and will be migrated to [Amazon CloudFront](https://aws.amazon.com/cloudfront/) in the future. LucidScale is used to diagram the entire AWS architecture to help staff understand the system and support future operations.

### **Expanding impact through cloud technology**

Support from the AWS Imagine Grant is helping Humane World for Animals transition from a distributed technology system to a unified system on AWS. The transition from beta architecture to AWS services is designed to promote global deployment capabilities and increase scalability as the team moves toward putting the application into production. The project will continue to advance the organization's mission through results based on actual effectiveness.

Humane World Apps is currently in the early stages of deployment in multiple countries, building on the success of the beta phase in India. The beta phase provided strong feedback on the application's effectiveness, demonstrating that technology can change animal welfare outcomes when properly deployed and scaled.

The web application serves as an administrative center where program managers design and monitor operations, provide feedback to vaccination teams, generate reports, and manage data across multiple regions. This centralized approach allows for better coordination and more efficient resource allocation across global programs.

### **Advice for nonprofit technology deployment**

Humane World for Animals offers valuable advice for nonprofits starting technology projects. First, they strongly recommend leveraging resources from Amazon, especially guidance from their technical solution architects. Second, they emphasize staying focused on the ultimate goal—when facing difficulties, remember the project's impact on the lives of street dogs. Finally, they emphasize the importance of implementing software development lifecycle processes early. Establishing source code control and error and requirement prioritization systems before starting is crucial, as implementing them midway consumed valuable time from the grant funding. Although the challenges were significant, the organization's commitment to improving animal welfare worldwide drove them to overcome obstacles and build a scalable, effective solution.

The success of Humane World Apps demonstrates the transformative power of cloud technology in addressing global challenges. By leveraging AWS services and the AWS Imagine Grant, Humane World for Animals has created a platform that not only improves animal welfare outcomes but also serves as a model for other nonprofits looking to scale their impact through technology.

As the organization continues to expand its reach and refine its platform, the lessons learned from this implementation will undoubtedly benefit the broader animal welfare community. The combination of innovative technology, dedicated volunteers, and strategic partnerships has created a powerful tool for creating positive change in the lives of animals worldwide.

The future of animal welfare programs lies in data-driven approaches that can adapt to local needs while maintaining global standards. Humane World Apps represents a significant step forward in this direction, demonstrating how technology can be harnessed to create more effective, efficient, and humane solutions to complex global challenges.

### **Authors**

![Author 1](/images/blog/blog2/author1.jpg)
*Stacy Stonich*

![Author 2](/images/blog/blog2/author2.jpg)
*Jules Marenghi*