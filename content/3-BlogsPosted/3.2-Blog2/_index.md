---
title: "Blog 2"
date: 2026-07-14
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# How ALS Geoanalytics’ LithoLens Revolutionizes Core Logging with Machine Learning on Amazon EKS

In mining and geoscience, **core logging** is a critical activity for recording the characteristics of drilled core samples. Specialists review images and document properties such as color, composition, structure, weathering, and other geological indicators. As the number of samples grows, manual processing becomes time-consuming, difficult to scale, and vulnerable to inconsistency across reviews.

**LithoLens by ALS Geoanalytics** illustrates how machine learning and cloud computing can support this workflow. Rather than treating images as isolated files, the solution accepts core photographs through an API, stores them durably, processes them with ML workloads running in containers, and associates the results with project metadata. With **Amazon Elastic Kubernetes Service (Amazon EKS)** as the container platform, the system can process many images concurrently and reduce turnaround time from weeks to hours, depending on workload volume and processing configuration.

## From manual logging to a digital workflow

A traditional workflow may involve photographing core samples, organizing files, recording observations, and asking specialists to review them one section at a time. This remains valuable because it captures expert judgment, but it introduces several challenges:

- a large image volume increases waiting time;
- project information and image files can become fragmented;
- processing capacity is difficult to increase during peak periods;
- repeated preprocessing tasks consume specialist and operational time;
- teams need visibility into whether an image is queued, processing, completed, or failed.

LithoLens places these steps into a trackable pipeline. Images enter through an API, are stored in Amazon S3, processed by ML workloads on EKS, and linked to project information in a relational database.

## High-level architecture

![LithoLens Architecture on AWS](/images/3-BlogsPosted/blog2.png)

The architecture can be viewed as three logical layers:

1. **API and identity**: Amazon API Gateway, AWS Lambda, and Amazon Cognito provide a secure application entry point.
2. **Compute and processing**: Amazon EKS runs containers that implement image-processing and machine-learning workloads.
3. **Storage, data, and observability**: Amazon S3 stores images and outputs, Amazon RDS manages metadata, and Amazon CloudWatch collects logs and metrics.

Separating these layers allows them to scale independently. Image volume can grow without requiring a redesign of the metadata model, and processing capacity can be adjusted without changing the image-upload experience.

## A typical core-image processing flow

A representative workflow looks like this:

1. A user signs in and is authenticated through Amazon Cognito.
2. The client submits an upload request or creates a processing job through API Gateway.
3. Lambda validates the identity, metadata, and input conditions.
4. The original image is stored in Amazon S3, while project, hole, sample, and job status information is stored in Amazon RDS.
5. The processing job is dispatched to the appropriate workload on EKS.
6. The ML container reads the input, performs preprocessing, and runs the application’s inference pipeline.
7. Results are written back to S3, and the searchable status is updated in RDS.
8. Logs, metrics, and failures are sent to CloudWatch for operational monitoring.

Keeping original images separate from metadata preserves the input for review or reprocessing. The relational database acts as the coordination and lookup layer, allowing users to identify the project associated with a result and see the current job state.

## The role of Amazon EKS

Amazon EKS provides a managed Kubernetes control plane while allowing the application team to focus on packaging and operating the processing containers. This is useful when the ML pipeline needs to:

- run multiple workloads concurrently;
- package model dependencies in reproducible containers;
- separate processing components into services or jobs;
- adjust compute capacity according to image volume;
- deploy controlled versions of models and processing pipelines.

Scalability is particularly important for image processing. As the number of jobs increases, additional processing capacity can be made available; when demand falls, unnecessary capacity can be reduced. Scaling should be combined with resource limits, job queues, and retry policies so that one malformed image or failed inference does not stop the entire pipeline.

EKS also creates a clear boundary between the API and the ML layer. The API does not need to perform expensive inference synchronously. It accepts the request and exposes job status, while the processing containers can be optimized, upgraded, or scaled independently.

## Amazon S3 for images and processing results

Core photographs can be large and may need to be retained for a long time. Amazon S3 is well suited for storing:

- original, unprocessed images;
- intermediate artifacts;
- processed outputs;
- artifacts needed for review and reprocessing.

Object keys should follow a consistent convention based on project, drill hole, sample, and processing job. Metadata must be governed carefully so that an image is correctly linked to its RDS record. Depending on project requirements, the platform may also use versioning, encryption, lifecycle rules, and retention policies to protect data and control storage cost.

## Amazon RDS for business data and metadata

Amazon RDS can store structured information such as:

- projects and users;
- drill-hole and sample information;
- upload and processing status;
- model or pipeline versions;
- S3 object references;
- summarized results and audit information.

Separating metadata from image files makes it faster to query jobs and status without scanning object storage. Recording the processing version also makes it possible to distinguish results created by different model or pipeline versions, which is important when comparing outputs or reproducing an analysis.

## API authentication and authorization

Amazon Cognito authenticates users before they access the application. API Gateway receives requests, while Lambda can perform additional checks such as:

- whether the user belongs to the requested project;
- whether the file and metadata have the expected format;
- whether the request is a duplicate;
- whether the job is within an allowed limit;
- whether the user can view the associated image or result.

Successful login should not be treated as permission to access every project. Authorization must be evaluated for the project, role, and requested operation. Core images and geological results may contain commercially sensitive information, so logs and API responses should avoid exposing data that is not required.

## Observability with Amazon CloudWatch

An ML pipeline can fail at many points: upload, authentication, object storage, job dispatch, container startup, inference, or result persistence. CloudWatch provides a central place to monitor:

- queued, running, and completed jobs;
- average processing time and jobs that exceed a threshold;
- failure rates by pipeline stage;
- CPU, memory, and cluster capacity;
- API, Lambda, and container errors;
- connectivity and operational health for S3 and RDS.

Measuring each stage separately helps identify whether the bottleneck is upload, database access, or ML compute. Alerts should be actionable—for example, a growing queue, an unusual error rate, or insufficient cluster capacity should lead to a clear operational response.

## Impact on geological operations

Parallel processing can substantially reduce waiting time and allow specialists to access results earlier. Digitization also makes the data easier to search, compare, and share than a workflow based on disconnected files.

Machine learning should still be treated as a decision-support capability rather than a complete replacement for geological expertise. Results should include status, provenance, and traceability so that uncertain cases can be reviewed manually and important conclusions can be validated.

## Deployment considerations

A similar system should address:

- consistent image quality, orientation, and input metadata;
- model and pipeline version management;
- retry, timeout, and dead-letter handling for failed jobs;
- file-size and upload-rate limits;
- data protection in transit and at rest;
- storage and compute cost controls;
- project- and role-based authorization;
- an audit trail for important outputs.

EKS scalability is most effective when the pipeline is asynchronous and jobs can run independently. If the API must wait for a long inference operation to finish, both the user experience and the system’s ability to absorb bursts will be constrained.

## Conclusion

LithoLens demonstrates how a data-intensive process such as core logging can be modernized by combining secure APIs, object storage, relational data, and machine learning on Kubernetes. Amazon EKS provides a flexible foundation for image-processing containers; S3 stores large data objects; RDS manages project and processing metadata; Cognito, API Gateway, and Lambda protect the access path; and CloudWatch provides observability.

The value of the architecture is not limited to faster model execution. When inputs, processing jobs, results, and operational logs are linked together, the organization gains a scalable and traceable pipeline that can be continuously improved—helping geoscience specialists make faster, more data-informed decisions.
