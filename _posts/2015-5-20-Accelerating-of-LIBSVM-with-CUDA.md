---
layout: post
title: Speeding up training process of libsvm with CUDA
---

These days I am doing a research about classifying abnormal beats from hours of heart beats from one person. One popular way is use Support Vector Machine to train a model and then classify the heart beats with the model trained. I have also done some work on CUDA one year ago, then I am thinking about uniting SVM with CUDA. 

###SVM package
The SVM package I am using is [libsvm](http://www.csie.ntu.edu.tw/~cjlin/libsvm/) by Chih-Chung Chang and Chih-Jen Lin.

I tested the accuracy of my algorithm with LIBSVM. The accuracy of the model in classify the various kinds of heart beat is over 98%.
###GPU-acclerated LIBSVM package
I google some open source on CUDA for accelerating SVM. [MKLab-ITI](http://mklab.iti.gr/project/GPU-LIBSVM) is by A. Athanasopoulos, A. Dimou, V. Mezaris and is already updated to compile with CUDA 5.5. Then I try to figure out the accelerating result of LIBSVM with CUDA.

During experiments, I found the cuda code only runs for cross-validation-enabled runs. If I didn't use **"-v X"**(X means the number you want in cross-validation), the single-threaded CPU version of the code would have run. 

The code on CUDA is shown:

```
void ckm( struct svm_problem *prob, struct svm_problem *pecm, float *gamma  )
{
   ...
   // Copy cpu vector to gpu vector
	status = cublasSetVector( len_tv, sizeof(float), &tva[trvei * len_tv], 1, g_vtm, 1 );
   		
	status = cublasSgemv( handle, CUBLAS_OP_N, ntv, len_tv, &alpha, g_tva, ntv , g_vtm, 1, &beta, g_DotProd, 1 );
    
	status = cublasGetVector( ntv, sizeof(float), g_DotProd, 1, DP, 1 );
    ...
}
```
I leave out some other codes, but I display the main code for CUDA here. cublasSgemv is for matrix multiplying. At this moment, I have no idea the matrix multiplying's exact function in the cross-validation process in LIBSVM. 

###Experiment
+ CPU: 2.7 GHz Intel Core i5
+ GPU: Tesla T10 Processor
+ SVM package: [libsvm](http://www.csie.ntu.edu.tw/~cjlin/libsvm/) 
+ CUDA open source code: [MKLab-ITI](http://mklab.iti.gr/project/GPU-LIBSVM)
+ Data:
  +  Test_Data1(34.7MB, features extracted from one-minute heart beats)
  +  Test_Data2(64.9MB, features extracted from two-minute heart beats)
 
This shows the time comparisons between CPU and GPU.

 ![Image Time](/images/time_comparison.jpg)

Only Test_Data1 is used. We can see that as the "v" increases, the time consuming on CPU increases much more than that on GPU. In "-v 3", time is almost the same but in "-v 12", the time on CPU is almost three times of the time on GPU.

This shows the time comparison on GPU between different Data.

![Image Time](/images/time_comparison2.jpg)

The Test_Data2 doubles Test_Data1, but time in "-v 3" on Test_Data1 almost four times on that on Test_Data2. As number of v increase, time gap is decreasing.

This is all I got until now. The following step is to 

+ accelerating the process of training through improving the CUDA code
+ accelerating the process of predicting with the model trained

If you are doing research on this, let's discuss to make it better.





 
 