## The project is now located at https://github.com/thaines/helit ##

This is basically a dumping ground for the code that I develop for my research. Unlike some researchers I am a firm believer that all source code must be published. Saying this I often omit the code that generated a papers results, simply because I don't have the bandwidth to distribute the data sets or the time to clean up what is often a horrific mess. Regardless, the actual algorithm is always available for people to run their own experiments with, and if you request testing code from me I'll probably provide it under the condition that your not allowed to laugh at me. It is all implemented using python, with inline C++ via scipy.weave for all the bits that need to be fast. You may find my personal website at http://thaines.com

Note that whilst I've stated an Apache License I tend to use a variety of licenses, typically BSD, Apache and GPL - check the headers. I generally put utility stuff as BSD, implementations of other peoples algorithms as Apache (Roughly summarised as BSD but with a longer license!) and my own research code as GPL. I do licensing on a directory by directory basis - you will not find multiply licensed files within a single directory, and the dependencies between directories obviously follow the relevant requirements. If you have good cause to request the code in another license feel free to ask - its not like I am attempting to make money out of this, so if its reasonable I will probably say yes.

It should be noted that I develop exclusively with 64 bit Linux, but being python most of this code should just work on other platforms. scipy and opencv are also used - most if not all modules use scipy, whilst opencv is mostly used by the test scripts rather than the modules themselves. Installing either of these libraries should be easy on mainstream platforms. scipy.weave is used by a few modules, and is a little tricky to install, as it requires a C++ compiler to be available. Whilst it works out of the box on most Linux installs, assuming you have a compiler, it can require some effort to get working elsewhere. Its not particularly hard under Windows - just google the instructions, but I have heard that it is a total bitch if you are using a mac. You might want to investigate running Linux inside a virtual machine instead (I've successfully done this with virtual box - http://www.virtualbox.org/), as it is just easier. Whilst I'm 90% sure that the code should work on 32 bit operating systems there is a risk that I have missed something, as I never test on something so antiquated. Also, I use soft links in some cases, which I don't think Windows supports - easiest solution is to duplicate the data.

This code is primarily for research, which is to say it is not of industrial standards. Some of it is sufficiently neat and robust that it would probably pass the needed testing regime however, once you slapped some sanity checking code in. Some of it is not however - depends mostly on how far out the next deadline was! Regardless, do report bugs to me so I can try to find the time to fix them. I'll also consider feature requests, so don't be afraid to ask.

If anyone wants to contact me for any of the above reasons, or any other good reason, then my email address is `<x>@<y>.com` where `<x>=thaines` and `<y>=gmail`.

I started this with a Subversion repository, which quickly got changed to a Mercurial repository, but then after that Google code added Git support, which I am much more familiar with. Unsurprisingly the repository now uses Git, after a certain amount of fiddling, which it should remain with for sometime as it is my favourite version control system at this instant. (Right now a lot of people seem to be finding this page by Googling for mercurial to git conversion - the tool you need is called hg-fast-export.sh. Note that after conversion your master will not match the remote repositories master - a rebase will fix that.)


Current contents:

**[utils](utils.md)**: (BSD)
Basic utilities shared between the various modules. Unusual stuff includes python code to change a python programs process name (Good for those killall's), a function that adds line numbers to scipy.weave code, which is essential if you want to debug them, and a multiprocess map replacement.

**[dp\_utils](dp_utils.md)**: (BSD)
A collection of C++ code for handling Dirichlet processes. Used by other modules in the system.

**[svm](svm.md)**: (Apache 2.0)
A support vector machine implementation, which only supports the traditional classification scenario. It is however implemented using some of the best available methods, and includes leave one out model selection with a decent set of kernel types and various useful defaults for when you just want to throw data at it. Its not as optimised as it could be, but is still plenty fast. I know there are already various other libraries for doing this with python, but my testing showed serious issues with all of them, specifically strange restrictions, lack of multiclass support and an inability to save models etc, not to mention absolutely dire interfaces and little documentation. This is only from a python point of view however - if you don't care which language you use then there are better libraries.


**[df](df.md)**: (Apache 2.0) A decision forest (random forest) implementation. It is extremely modular, for future expandability; current version is rather limited, but I intend to grow it until it is one of the best. Very flexible, with support for incremental learning and both continuous and discrete features. Currently only supports classification with a limited set of test generation techniques, which is basically the standard feature set for a typical random forest implementation. Currently it is python with numpy/scipy only; code is well commented and should be relatively easy to understand.


**[lda\_gibbs](lda_gibbs.md)**: (Apache 2.0)
A latent Dirichlet allocation implementation using Gibbs sampling. Written as a learning exercise and so not the best organised code, but fairly well commented, so has possible educational value. Uses scipy.weave and is hence reasonably fast.

**[lda\_var](lda_var.md)**: (Apache 2.0)
A latent Dirichlet allocation implementation using the mean field variational method. Again, written as a learning exercise. Code is disturbingly neat, plus short, and probably quite educational. It is in straight python without any inline c++, so it is easy to run - it could of course be faster using inline code, but still manages to be an order of magnitude faster than the Gibbs approach, which does use inline C++.

**[rlda](rlda.md)**: (GPL v3)
My region LDA implementation - see the paper 'Video Topic Modelling with Behavioural Segmentation' by T. S. F. Haines and T. Xiang (Downloadable from my website.). A topic model that includes a behavioural segmentation within the same model, which is simultaneously solved. Designed to be good at analysing traffic data by virtue of it inferring the regions of the input where each activity occurs, giving it a better generalisation capability.

**[dhdp](dhdp.md)**: (Apache 2.0)
A Dual Hierarchical Dirichlet processes implementation, using Gibbs sampling. Also includes the ability to switch off the document clustering and obtain a HDP implementation. Its rather complex - a lot is going on, and its design is arguably not the best. Makes extensive use of scipy.weave as most of the code is actually in C++.

**[ddhdp](ddhdp.md)**: (GPL v3)
My Delta-Dual Hierarchical Dirichlet Processes implementation, from the paper 'Delta-Dual Hierarchical Dirichlet Processes: A pragmatic abnormal behaviour detector' by T. S. F. Haines and T. Xiang (Can be obtained from my website.). It shares a lot of code with the above dhdp implementation unsurprisingly, making similarly extensive use of scipy.weave. Also, if dhdp is complex this is bordering on the insane - its an awful lot of very complex code, compliments of a lot of variables that need to be Gibbs sampled.


**[gcp](gcp.md)**: (Apache 2.0)
The code associated with my [Gaussian conjugate prior cheat sheet](http://thaines.com/content/misc/gaussian_conjugate_prior_cheat_sheet.pdf). Basically an implementation of exactly what you would expect, though it has a few extra features/tweaks since I uploaded it as an archive to download from my website due to it seeing some real world usage. It of course has a conjugate prior class, with the ability to provide new samples and draw a Gaussian or draw a Student-t distribution that is the probability of a sample drawn from the Gaussian drawn from the prior, but with the Gaussian integrated out. Additionally classes to represent Gaussian, Wishart and Student-t distributions are provided, as well as an incremental Gaussian fitter. All written in pure python.

**[ms](ms.md)**: (Apache 2.0)
Provides a mean shift implementation, but also includes kernel density estimation and subspace constrained mean shift using the same object, such that they are all using the same underlying density estimate. Includes multiple spatial indexing schemes and kernel types, including one for directional data. Clustering is supported, with a choice of cluster intersection tests, as well as the ability to interpret exemplar indexing dimensions of the data matrix as extra features, so it can handle the traditional image segmentation scenario.

**[kde\_inc](kde_inc.md)**: (Apache 2.0)
A simple incremental kernel density estimation module with Gaussian kernels, that uses a fixed number of kernels and greedily merges kernels together when it exceeds that cap. Also includes leave one out optimisation of the kernel size, in terms of a symmetric precision matrix. Nothing special really - I only implemented this so I could match the experimental setup of another paper. Its simple and neat however, and I guess could be useful when the fixed time property matters, though if that is really the case you would probably want a C version, rather than this python-only snail.

**[gmm](gmm.md)**: (Apache 2.0)
A fairly standard Gaussian mixture model library, that uses k-means for initialisation followed by EM to fit with the actual model. Only supports isotropic kernels, but then non-isotropic kernels can cause stability issues with EM. Has a Bayesian information criterion (BIC) implementation, for automatically selecting the number of clusters. Includes some very nice, and particularly fast, k-means implementations, in case that is all you want. Makes extensive use of scipy.weave.

**[dpgmm](dpgmm.md)**: (Apache 2.0)
A Dirichlet process Gaussian mixture model, implemented using the mean field variational technique. Its about as good as a general purpose density estimator can get, though it suffers from a heavy computational and memory burden. Code is in pure python, depends on the gcp module and it is very neat and reasonably well commented - speed is reasonable for the method as the code vectorises well.

**[p\_cat](p_cat.md)** (Apache 2.0)
A bunch of probabilistic classifiers, using some of the above density estimation methods. Does it all via a standard interface, so they can be swapped out, and includes incremental learning and, somewhat unusually, the probability of a sample belonging to an unknown class, as calculated under a Dirichlet process assumption (This last feature is for a paper.).

**[dp\_al](dp_al.md)**: (GPL v3)
An active learning method of mine, that uses Dirichlet processes. The relevant paper is 'Active Learning using Dirichlet Processes for Rare Class Discovery and Classification' by T. S. F. Haines and T. Xiang, from BMVC 2011 (Can be obtained from my website.). A very simple algorithm that works very well when faced with the competing goals of finding new categories, particularly rare ones, and refining the classification boundaries of existing categories. It is a bit absurd me putting code up, given that implementation is so easy, so the main value here is the test code and the framework that allows for comparison with a bunch of other active learning algorithms that are also implemented within this module.


**[smp](smp.md)**: (GPL v3)
This Sparse Multinomial Posterior library solves the rather unusual problem of estimating a multinomial given draws from it, when those draws are sparse. That is, when the counts for some of the entries are missing. Generates the expected value of the multinomial being estimated. Uses scipy.weave.


**[video](video.md)**: (GPL v3)
A node based system for processing video, that includes various computer vision algorithms, e.g. optical flow, background subtraction. This last one is my own algorithm: 'Background Subtraction with Dirichlet Processes' by Tom SF Haines & Tao Xiang, ECCV 2012. Uses OpenCV, scipy and also compiles C code and uses OpenCL when available.