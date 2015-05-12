---
layout: post
title: The Framework of SCI paper
---
###1.背景#
+	穿戴式裝置臨床研究的重要性
+	cloud computing and storage在wearable device中的作用及需求
+	雲端的應用現狀及其優勢（效率等方面）

###2.方法(Algorithm)#
+	Algorithm One:
	+	HRV時域和頻域參數的計算
	+	已完成使用cuda對R-peak探測的加速工作
+	Algorithm Two:
	+	對Normal beat和abnormal beat進行分類
	+	已完成五種主要beat的分類（使用SVM）
	+	待完成工作：
		+	增量學習：在MITBIH數據庫訓練出的模型上加入待測病人自身數據，反復自我學習，得到更精確的模型
		+	加入unknown beat：在分類模型中，若待測beat與我們訓練模型中beat的特徵值相似度低於一定閾值，可以將其歸類爲unknown beat
		+	需要在matlab上先驗證演算法的正確性，然後將算法移植到我們的平臺上
+	Algorithm Three:
	+	ST segment: ST segment在我們一些病症診斷上有重要的作用
	+	這一部分工作在前兩項工作結束后進行

###3.實驗#
+	**正確性**：波性分類的正確性。
+	**執行效能**：
	+	R-peak檢測，頻域時域參數計算的時間大幅縮短（預計）
	+	abnormal檢測的速度也大幅提高
	+	要將我們雲端上計算大量的數據的時間與沒有經過CUDA加速的時間進行對比。凸顯我們的優勢。
	+	優化GPU存儲方案，不同存儲方案之間程序執行速度進行對比，得到最優的存儲方案。


###4.其他#
+	Data來源：
	+	MITBIH ANATHYMIA 
	+	AHA ECG database
+	Algorithm:
	+	R-peak檢測算法（curve length transform 演算法）
	+	SVM支持向量機進行beat的分類
+	實驗的平臺
	+	暫時計劃的平臺是在裝有LINUX CENTOS的TK1