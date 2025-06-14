### Elastic Block Storage { EBS }

* Amazon EBS is a block-level storage service designed specifically for EC2 instances.
* An important benefit of EBS is its independence from EC2 instances. You can detach an EBS volume from one instance and reattach it to another without data loss.
* EBS volumes are provisioned within a single availability zone (AZ), ensuring built-in redundancy within that zone.
* If one physical device fails, the volume remains operational. However, if the entire AZ experiences an outage, the data on the volume can be lost.
* This means that an EC2 instance and its attached EBS volume must reside in the same AZ.

#### EBS Volume Types
* General Purpose SSD (gp2 and gp3)
  * gp3 & gp2 are normal SSDs .
  * These volumes deliver a balanced price/performance ratio ideal for a wide range of workloads such as virtual desktops, medium-sized databases, and interactive applications.
    
* Provisioned IOPS SSD (io1, io2, and io2 Block Express)
  * Designed for I/O-intensive and low-latency workloads.
  * IO1/IO2: Both offer high performance, with IO2 delivering higher durability (99.999% compared to approximately 99.8%–99.9% for IO1).
  * IO2 Block Express: Supports extreme workloads with maximum volume sizes up to 64 TB, IOPS up to 256,000, and throughput up to 4000 MB/s.

* Hard Disk Drive (HDD) Volumes
  * Suitable for less frequently accessed data

* Magnetic Volumes
  * Ideal for workloads with small, infrequently accessed datasets where high performance is not essential. Typically, these volumes deliver around 100 IOPS on average, with burst capabilities to a few hundred IOPS, and range in size from 1 GB to 1 TB.

#### EBS Operation Workflow

* Create an EBS Volume: Provision your volume within the desired AZ.
* Attach to an EC2 Instance: The volume is then presented as a block device.
* Data Migration within the Same AZ: Detach the volume from one instance and attach it to another within the same AZ if needed.

* For data migration between regions, the process is analogous:
  * Take a snapshot (stored in an S3 bucket in the source region).
  * Copy the snapshot to an S3 bucket in the destination region.
  * Create a new volume from the copied snapshot and attach it to an EC2 instance.

 ![image](https://github.com/user-attachments/assets/504d425d-7394-438f-9d3e-35c3b7e76e6f)
 

