---
layout: jclouds
title: Quick Start - SoftLayer CloudLayer Compute
---

# Quick Start: SoftLayer CloudLayer Compute

This page helps you get started with jclouds API with SoftLayer CloudLayer Compute and to create Cloud Compute Instances

1. Sign up for [SoftLayer Portal](https://control.softlayer.com/).
2. Get your **User** and **API Access key** by going to this [page](https://manage.softlayer.com/Administrative/apiKeychain).
3. Ensure you are using a recent version of Java 6 or greater.
4. Setup your project to include `softlayer`.
	* Get the dependency `org.jclouds.provider/softlayer` using jclouds [Installation](/documentation/userguide/installation-guide).
5. Start coding.

## SoftLayer CloudLayer Compute

{% highlight java %}
// Get a context with softlayer that offers the portable ComputeService API
ComputeServiceContext ctx = ContextBuilder.newBuilder("softlayer")
                      .credentials("User", "Api Access key")
                      .modules(ImmutableSet.<Module> of(new Log4JLoggingModule(),
                                                        new SshjSshClientModule()))
                      .buildView(ComputeServiceContext.class);

ComputeService compute = ctx.getComputeService();

// List availability zones
Set<? extends Location> locations = compute.listAssignableLocations();

// List nodes
Set<? extends ComputeMetadata> nodes = compute.listNodes();

// List hardware profiles
Set<? extends Hardware> hardware = compute.listHardwareProfiles();

// List images
Set<? extends org.jclouds.compute.domain.Image> image  = compute.listImages();

// Create nodes with templates
Template template = compute.templateBuilder().osFamily(OsFamily.UBUNTU).build();
// by default, all the nodes created by jclouds will have `jclouds.org` as the domain name. 
// If you want to edit that default value, you can specify something like:
// Template template = compute.templateBuilder().options(SoftLayerTemplateOptions.Builder.domainName("yourDomainName"))
//                                              .osFamily(OsFamily.UBUNTU)
//                                              .build()
Set<? extends NodeMetadata> groupedNodes = compute.createNodesInGroup("myGroup", 2, template);

// Reboot images in a group
compute.rebootNodesMatching(inGroup("myGroup"));

// Destroy nodes in a group
Set<? extends NodeMetadata> destroyed = compute.destroyNodesMatching(inGroup("myGroup"));

{% endhighlight %}

5. Validate via the [SoftLayer Portal](https://manage.softlayer.com/CloudLayer/computing)