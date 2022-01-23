---
title: reflect | epsrc iaa
date: 2022-01-08
---

### reflect - **an epsrc iaa project exploring the impact of wearable data on personalised decision support**

![wearable](/img/wearable.png "wearable")

### about

The [epsrc `consult` project](https://gow.epsrc.ukri.org/NGBOViewGrant.aspx?GrantRef=EP/P010105/1) developed a [decision-support system](https://kclhi.org/consult/demo/?a=UGU2YmFxRUQ6dWtlN2JQRXk=) characterised by its integration of computational argumentation (a form of AI) with wearable device data.

`reflect` is an [epsrc iaa project](https://kclpure.kcl.ac.uk/portal/en/projects/reflect-wearable-sensors-for-personalised-decision-support(a572899d-7799-40fa-a141-4e3efc79b7ca).html) that aims to generalise and optimise the wearable data collection logic developed within `consult`, in order to provide this data to other AI-based decision-support systems.

In doing so, the impact of wearable data on the operation of these systems, and thus on personalised patient healthcare, can be further explored.

&nbsp;
*** 
### people

| [![martin chapman - pi](/img/people/chapman.jpg "martin chapman - pi")](https://martinchapman.co.uk) | [![vasa curcin - co-pi](/img/people/curcin.jpg "vasa curcin - co-pi")](https://kcl.ac.uk/people/vasa-curcin) | [![abigail g-medhin - data scientist](/img/people/g-medhin.jpg "abigail g-medhin - data-scientist")]() | [![abhiram ravikuar - research software engineer](/img/people/ravikumar.jpg "abhiram ravikumar - research software engineer")]() |
| - | - | - | - |
| **Dr. Martin {{< line_break >}} Chapman** | **Dr. Vasa {{< line_break >}} Curcin** | **Abigail {{< line_break >}} G\-Medhin** | **Abhiram {{< line_break >}} Ravikumar** |
|   PI   | Co-PI | Data Scientist | Research Software Engineer |

### partners

|[![metadvice](/img/partners/metadvice.jpg "metadvice")](https://www.metadvice.com/)|
| - |

### devices

|[![withings](/img/devices/withings.jpg "withings")](https://www.withings.com/uk/en/)|[![garmin](/img/devices/garmin.jpg "garmin")](https://www.garmin.com/en-GB/)|
| - | - |

&nbsp;
*** 
### software

To generalise  `consult`'s wearable data logic, `reflect` packages this logic as a set of new [components](#components) - developed on top of a new [software stack](#stack) - that are designed to be deployed to a newly defined [server architecture](#architecture).
These components then [combine](#flow) to collect, and provide access to, wearable data.  

&nbsp;

#### components

`reflect`'s core components are as follows:

- [`user`](https://github.com/kclreflect/user) (microservice) - presents a patient-optimised interface that allows users, or a GP on their behalf, to connect their wearable devices - via the device vendor - with `reflect`
- [`notify`](https://github.com/kclreflect/device/-/tree/main/notify) (function) - receives data from the wearable devices, via the device vendors' servers
- [`convert`](https://github.com/kclreflect/data/-/tree/main/convert) (function) - standardises the data received from multiple vendors to [`fhir`](https://www.hl7.org/fhir/), to ensure a unified data format is presented to third-party systems
- [`api`](https://github.com/kclreflect/api) (microservice) - allows decision-support systems to access the collected data

View [all `reflect` software repositories](https://github.com/kclreflect).

&nbsp;

#### stack

`reflect`'s components are built on top of the following software stack:

{{< stackshare data-theme="light" data-layers="1,2,3,4" data-stack-embed="true" href="https://embed.stackshare.io/stacks/embed/a25989f945011285c2d47a74255c51" >}}

&nbsp; 

#### architecture

`reflect`'s software components are designed to be deployed to a Kubernetes cluster (or to Minikube for testing), to ensure the scalability demands of external reasoners are met.
This cluster can be realised by a cloud provider such as AWS as follows:

{{< figure src="/img/software/architecture.png" >}}

Here, a VPC provides both public and private subnets, with the latter holding the replicated nodes to which `reflect`'s software components are deployed.
These components communicate via an internal load balancer, while external requests (such as incoming device data) are received via a web-facing load balancer.

This configuration can be viewed [here](https://github.com/kclreflect/config).

&nbsp; 

#### flow

To provide decision-support systems with access to wearable device data, `reflect`'s components combine as follows:

**1. device connection**

{{< figure src="/img/software/authenticate.png" >}}

A user navigates to the `reflect` web application in order to associate a unique patient ID (such as an NHS number) with the data collected from various devices, which they proceed to authorise via each device vendor's server.

**2. data storage**

{{< figure src="/img/software/add.png" >}}

When new data is collected by a patient's device, it is forwarded to the `reflect` servers. Here, the data is securely cached before being standardised to `fhir`, and then stored in a [HAPI FHIR](https://hapifhir.io/) server.

**3. data retrieval**

{{< figure src="/img/software/get.png" >}}

Using a patient ID (collected separately), a third-party decision-support system calls the `reflect` servers for the latest data available on that patient.

&nbsp;

#### screenshots

{{< figure src="/img/screenshots/signup.png" >}}
{{< figure src="/img/screenshots/services.png" >}}
