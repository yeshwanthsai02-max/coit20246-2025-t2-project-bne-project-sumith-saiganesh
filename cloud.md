# 4.2 Cloud Services

In the current situation, the organization keeps all types of data and servers at headquarters; however, it is shifting some parts to the cloud. Therefore, this task explores, evaluates, and recommends multiple cloud services better suited for operations.

## Cloud Server Comparison

| Feature | Microsoft Azure | Amazon Web Services (AWS) |
|----------|------------------|---------------------------|
| **Data-center location** | Australia East (Sydney), Australia Southeast (Melbourne) | Access Point Southeast-2 (Sydney) |
| **Virtual machine estimation** | D4s v3 – 4 vCPU, 16 GB RAM | m5.xlarge – 4 vCPU, 16 GB RAM |
| **Operating System** | Windows Server 2022 (64-bit) | Windows Server 2022 (64-bit) |
| **Storage size (each VM)** | 100 GB | 100 GB |
| **Virtual machine rent (monthly)** | AUD $227 | AUD $221 |
| **5-year plan cost** | ≈ AUD $95,369 | ≈ AUD $92,830 |
| **Hybrid Integration** | Seamless with Windows Server, Active Directory, Microsoft 365, Azure Arc | Possible via AWS Directory Service and AD Connector (less native) |

### Azure VM Cost Estimation

| Provider | Region | Instance Type | vCPU | RAM | OS | Storage Type | Storage (GB) | Cost (AUD/month) | Cost (AUD/5yr per VM) |
|-----------|---------|----------------|------|-----|----|---------------|---------------|------------------|-------------------------|
| Microsoft Azure | Australia East (Sydney) | D4s_v3 | 4 | 16 | Windows Server 2022 | SSD Managed Disks | 100 | 239.59 | 1677.11 |

### AWS VM Cost Estimation

| Provider | Region | Instance Type | vCPU | RAM | OS | Storage Type | Storage (GB) | Cost (AUD/month) | Cost (AUD/5yr per VM) |
|-----------|---------|----------------|------|-----|----|---------------|---------------|------------------|-------------------------|
| Amazon Web Services (AWS) | ap-southeast-2 (Sydney) | m5.xlarge | 4 | 16 | Windows Server 2022 | EBS gp3 SSD | 100 | 224.44 | 1571.07 |

## Recommendations

- **Azure** is recommended for organizations heavily using Windows or SQL Server due to smooth hybrid integration, simplified migration, and data management.  
- **AWS** is recommended when cost optimization is critical, as it offers slightly cheaper on-demand services.  
- For **small to medium organizations** using Microsoft products, Azure provides maximum flexibility and better integration.

## References

- Juhasz, Z. (2021). *Quantitative cost comparison of on-premise and cloud infrastructure based EEG data processing.* Cluster Computing, 24(3), 625–641. [SpringerLink](https://doi.org/10.1007/s10586-020-03141-y)
- Cho, K., & Bahn, H. (2020). *A cost estimation model for cloud services and applying to PC laboratory platforms.* Processes, 8(1), Article 76. [MDPI](https://doi.org/10.3390/pr8010076)
- Dimitri, N. (2020). *Pricing cloud IaaS computing services.* Journal of Cloud Computing, 9, Article 14. [SpringerOpen](https://doi.org/10.1186/s13677-020-00161-2)
- Khan, S. U., & Ullah, et al. (2016). *Challenges in the adoption of hybrid cloud: an exploratory study using systematic literature review.* The Journal of Engineering, 2016(5), 107–118. [IET Research Journals](https://doi.org/10.1049/joe.2016.0089)
- Soveizi, N., Turkmen, F., & Karastoyanova, D. (2022). *Security and privacy concerns in cloud-based scientific and business workflows: A systematic review.* [arXiv](https://arxiv.org/abs/2210.02161)

