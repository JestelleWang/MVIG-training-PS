# What's the Role of Image Matting in Image Segmentation?

Image Matting is the key technology in image processing, video editing, and film-making applications. With the fast development of modern information technology, image matting has gained increasing interests from both academic and industrial communities.  

## Introduction
### What is Image Matting?

Image matting refers to the problem of extracting in- teresting targets from a static image or a video sequence, which has played an important role in many image and video editing applications. The inverse procedure of image matting is known as image compositing. The key issue involved in any image matting algorithm is to determine both full and partial pixel coverage of the targets in the image, also known as *pulling a matte*.

**matting equation**

```
C(p) = a(p)F(p) + (1 - a(p))B(p) 
```

where  

- C : observed image
- F : foreground image
- B : background image
- p = (x, y) : image coordinate
- a(p) : the pixels' foreground opacities

The image matting problem is inherently an under-constrained problem. Due to the ill-posed property of the matting problem, to achieve good estimations for the unknowns, user input or prior assumptions are usually used in current matting algorithms. The two representative user input methods are trimap and strokes.
 
### Why Introduce Image matting Technology?

To realize the segmentation for some special object which is narrower than one pixel or includes some translucent objects, some traditional image segmentation algorithms usually cannot seem to get started, while digital image matting technology for transparency/elongated objects can be the very right solutions. Given an image *Cp*, the objective of image matting is to reconstruct/recover/extract *Fp*, *Bp* and *αp* of matting equation.

### Image Matting vs Image Segmentation

For matting equation, if the alpha matte αp is constrained to (0,1), it represents an image matting or compositing problem. If the value of the alpha matte is 0 or 1, this equation will simplify to a typical image segmentation prob- lem, where each pixel belongs to either the foreground or the background. Obviously, the difference between image matting and segmentation comes from the value of αp. In image segmentation, the pixel only needs to be judged whether it belongs to the foreground or the background. In image matting, the foreground opacity of the pixel is also needed to be precisely estimated. Technically, image matting is a more challenging task than image segmentation. That is also one of the reasons why existing matting algorithms usually introduce an interactive operation such as trimap and strokes. With the user interaction and input, the dimension of the solution space in the matting problem can be greatly decreased, and it can lead the matting algorithms to generate user-desired results.

> **trimap**  
> Y. Mishima, “Soft edge chroma-key generation based upon hexocta- hedral color space,” U.S. Patent 5, 355, 174, 1993.  > Y. Chuang, B. Curless, D. H. Salesin, and R. Szeliski, “A Bayesian Approach to Digital Matting,” in Proc. IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2001, pp. 264-271.

> **strokes**  
> Y. Guan, W. Chen, X. Liang, Z. Ding, and Q. Peng, “Easy matting: A stroke based approach for continuous image matting,” in Proc. Eurographics, 2006, pp. 567-576.  > Y. Zheng, C. Kambhamettu, J. Yu, T. Bauer, and K. Steiner, “Fuzzy- Matte: A Computationally Efficient Scheme for Interactive Matting,” in Proc. IEEE Conference on Computer Vision and Pattern Recogni- tion (CVPR), 2008, pp. 1-8.

## Interactive Mode of Image Matting

### Trimap

To properly extract semantically meaningful foreground objects, the user can manually label an input image as three parts before matte pulling, i.e., foreground, background and unknown regions in the image. For a given trimap, the image matting problem is thus simplified to estimate the foreground colors, background colors, and alpha values for the pixels in unknown regions based on the known foreground and background pixels.

### Strokes

Compared with the trimap method which needs to mark all image regions, the strokes method only needs to specify a few foreground and background scribbles in appropriate image regions. In the strokes-based algorithms, these marked scribbles are considered as the input and used to extract the alpha matte. In comparison to trimap-based algorithms, strokes-based algorithms demand fewer user interaction and operation.

### Comparison between the Trimap and Stokes Method

For the trimap method, the crucial step is to construct an accurate trimap for the desired foreground in the given image. Generally, the unknown regions around the foreground boundaries are expected to be specified as fine as possible to achieve satisfied matting results. That is because an accurate trimap can provide more detailed information about the background and foreground, and thus improves the matting accuracy. A major advantage of the trimap method is that the drawing of the trimap is a natural operation, and allows the user intuitively determine where to put the labels. However, generating a high quality trimap is usually a very time-consuming operation, especially for images with plentiful of details such as hairs and leaves.

In comparison, the strokes method gives more freedom since it requires no strict interactive operation. But results by the strokes-based image matting methods are usually dependent to the locations of the user specified scribbles. Making the problem more complicated, the user may not realize what kind of labels can achieve better matting results, and unsuitable labeling operations can degrade the matting quality. In addition, unknown image regions by the strokes methods are usually larger than that of the trimap methods, and thus increase the whole computation.

In terms of application purposes, the trimap method is appropriate for the matting situations where high quality matting is demanded, and the strokes method is suitable for matting occasions where no high accuracy matting is re- quired but free-style user interaction is preferred. Therefore, one important tendency for image matting is to obtain an optimal tradeoff between the accuracy of the matting results and the amount of user interaction required.

## Blue Screen Matting

Blue screen matting is a very practical means for image matting and has been widely used in film and publication industries. It is also the first technique that can be used for live action matting. In the blue screen matting methods, the matting problem is simplified by photographing the foreground object against one or multiple backgrounds with known constant colors. However, the under-constrained problem still exists, as the blue background is usually affected by the shadow casting and lighting conditions.

## Natural Image Matting

Because natural image matting deals with images with arbitrary backgrounds, it has more applications for both industrial and personal purposes. In this survey, we will focus on introducing natural image matting and classifying the methods into four categories: sampling-based, propagation- based, combination of sampling and propagation, and ma- chine learning based approaches.

### Sampling-based Image Matting

The sampling-based image matting methods usually con- sider a local region around one pixel. And the alpha parameter can be estimated by analyzing color distributions of its neighboring pixels, as well as the foreground or background color of the unknown pixels. Classical sampling-based image matting algorithms focus on how to model the relations between the neighboring samples and the alpha parameter. Recently, more attentions have been paid to the optimal selection of ‘good’ foreground and background samples. A variety of representative sampling-based image matting algorithms appeared around 2000, including the Knockout method, the approach by Ruzon and Tomasi, and the Bayesian matting method. These typical sampling-based techniques use the trimap as user input.

### Propagtion-based Image Matting

Due to the difficulties encountered by sampling-based techniques, the propagation-based techniques are proposed. The basic idea behind is to utilize the neighboring pixel affinities to constrain the matting equation. Various affinities between pixels and the user input are used as the constrain conditions. The alpha matte can be estimated by propagating the constraints from the known regions to the unknown regions. The propagation-based techniques make use of the local correlation among neighboring pixels with fewer user input requirements. Two key issues are involved in the propagation-based matting algorithms: 

- a) the definition of relation between pixels
- b) how to model the propagation. 

### Combination of Sampling-based and Propagation-based

As discussed above, sampling-based and propagation-based techniques both have their own advantages and disadvantages. Sampling-based techniques work well in unknown smooth regions, but may fail in complex scenes where the appropriate samples are difficult to be collected.Propagation-based techniques focus on the relations among neighboring pixels. This kind of techniques can work well with complicated images where the underlying assumptions stand. Recent algorithms tend to combine these two kinds of techniques into an energy function and to solve the matting problem in a global optimization process. These techniques are comparable to the binary image segmentation algorithms that solve the segmentation problem by optimizing some global criteria which combine intra-region consistency and inter-region dissimilarity.

### Learning-based Image Matting

Different from the sampling-based and propagation-based matting approaches, the learning-based matting techniques do not use the samples or strong assumptions directly. They are usually implemented via a training procedure, and esti- mate the matting parameters in a learning-based framework.

> **REFERENCE**  
> What’s the Role of Image Matting in Image Segmentation? *Qingsong Zhu∗, Member, IEEE, Pheng Ann Heng, Senior Member, IEEE, Ling Shao, Senior Member, IEEE, Xuelong Li, Fellow, IEEE* Proceeding of the IEEEInternational Conference on Robotics and Biomimetics (ROBIO) Shenzhen, China, December 2013



