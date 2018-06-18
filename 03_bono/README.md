
<h2>AJACS51/bono</h2>

<p><span style="font-size:36px;display:inline-block;line-height:130%;text-indent:0px">RおよびBioconductorを使ったバイオデータ解析</span></p>
<p>講習では【実習】を皆さんとやっていきます。早くできてしまった人は、【発展】をやってみたりしてください。
Rのインストールは以下の統合TVを参考に行っておいてください。</p>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>参考：統合TV<a href="http://togotv.dbcls.jp/" rel="nofollow"><img src="http://togotv.dbcls.jp/ja/wp-content/uploads/2013/03/togotv_banner.png" alt="http://togotv.dbcls.jp/" /></a>
<ul class="list2" style="padding-left:16px;margin-left:16px"><li><a href="http://togotv.dbcls.jp/20090313.html" rel="nofollow">統計解析ソフト「R」の使い方 導入編</a></li>
<li><a href="http://togotv.dbcls.jp/20090618.html" rel="nofollow">統計解析ソフト「R」の使い方 導入編(MacOSX)</a></li>
<li><a href="http://togotv.dbcls.jp/20090319.html" rel="nofollow">統計解析ソフト「R」の使い方 正規化編</a></li>
<li><a href="http://togotv.dbcls.jp/20091219.html" rel="nofollow">統計解析ソフト「R」の使い方 ヒートマップ編</a></li>
<li><a href="http://togotv.dbcls.jp/20111107.html" rel="nofollow">統計解析ソフト「R」での立廻り</a></li></ul></li></ul>
<hr class="full_hr" />
<p>目次</p>
<div class="contents">
<a id="contents_1"></a>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li><a href="#d16282c7">  0. はじめに </a></li>
<li><a href="#ib0f08d6">  1. パイチャート </a></li>
<li><a href="#z73b23bc">  2. 階層的クラスタリング </a></li>
<li><a href="#uca2d586">  3. Affymetrixデータ正規化 </a></li>
<li><a href="#od88c1b7">  4. 主成分分析(PCA) </a></li>
<li><a href="#xbad7386">  5. NGS解析(cuffdiffの結果可視化) </a></li>
<li><a href="#sc4355c8">  6.最後に: R Graphical Manualのすゝめ </a></li></ul>
</div>

<hr class="full_hr" />
<h3 id="content_1_0"><a id="d16282c7" href="" title="d16282c7"><span class="sanchor">_</span></a> 0. はじめに  </h3>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>【実習0】Rをインストールしなさい。
<a name="plugin_fold_anchor1"></a>
<div class="plugin_fold_title_plus" onclick="return plugin_fold_onclick(this,event,'plugin_fold_anchor1')"><p>←こたえは左の+マークをクリックすると出てきます</p>
</div>
<div class="plugin_fold_body"><p><a href="http://cran.ism.ac.jp/" rel="nofollow">http://cran.ism.ac.jp/</a> からダウンロードしてインストールしてください</p>
</div></li>
<li>【発展0】Rを快適に使うにはRstudioというソフトウェアがある。Rstudioをググッて見つけ、インストールしなさい。
<a name="plugin_fold_anchor2"></a>
<div class="plugin_fold_title_plus" onclick="return plugin_fold_onclick(this,event,'plugin_fold_anchor2')"><p>←こたえは左の+マークをクリックすると出てきます</p>
</div>
<div class="plugin_fold_body"><p><a href="http://www.rstudio.com/" rel="nofollow">http://www.rstudio.com/</a> からダウンロードしてインストールしてください</p>
</div></li>
<li>【発展1】今回使うRのコードやデータは<a href="https://github.com/bonohu/AJACS51" rel="nofollow">githubのサイト</a>にあります。そこのデータをまとめてダウンロードするには、UNIXのコマンドラインで
<pre>git clone https://github.com/bonohu/AJACS51</pre>
を実行しなさい。現在居るディレクトリ(current working directory)にどういった変化が起きたか?
<a name="plugin_fold_anchor3"></a>
<div class="plugin_fold_title_plus" onclick="return plugin_fold_onclick(this,event,'plugin_fold_anchor3')"><p>←こたえは左の+マークをクリックすると出てきます</p>
</div>
<div class="plugin_fold_body"><p>AJACS51というディレクトリが作成され、それ以下にgithubにあるファイルがすべてダウンロードされた</p>
</div></li></ul>

<h3 id="content_1_1"><a id="ib0f08d6" href="" title="ib0f08d6"><span class="sanchor">_</span></a> 1. パイチャート  </h3>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>【実習1】Rを起動し以下のファイルに記述されたRのコマンド(<strong>01piechart.r</strong>)を実行しなさい。データファイルとして必要な<strong>srabystudy.txt</strong>をgithubのサイトから<a href="https://raw.githubusercontent.com/bonohu/AJACS51/master/srabystudy.txt" rel="nofollow">ダウンロード</a>し、現在作業しているディレクトリにおいておく必要があります。実行後、そのディレクトリに<strong>pie1.png</strong>というファイルが新たに生成されていることを確認しなさい。</li></ul>
<pre>png(&quot;pie1.png&quot;)
dat &lt;- read.delim2(&quot;srabystudy.txt&quot;, header=F)
names(dat) &lt;- c(&quot;Study&quot;, &quot;freq&quot;)
dat &lt;- cbind(dat, serial=seq(dim(dat)[1]))
dat2 &lt;- dat[ dat$freq != 0, ]
pie(dat2$freq, labels=paste(dat2$Study, dat2$freq), col=rainbow(dim(dat[1])[dat2$serial]), main=&quot;SRA by Study&quot;)
dev.off()</pre>
<a name="plugin_fold_anchor4"></a>
<div class="plugin_fold_title_plus" onclick="return plugin_fold_onclick(this,event,'plugin_fold_anchor4')"><p>←左の+マークをクリックするとこのスクリプトの経緯が…</p>
</div>
<div class="plugin_fold_body"><p>Bono H et al. &quot;Systematic Expression Profiling of the Mouse Transcriptome Using RIKEN cDNA Microarrays&quot; Genome Res 2003の<a href="http://genome.cshlp.org/content/13/6b/1318/F1.expansion.html" rel="nofollow">図1</a>を作成するために使ったものです。実際には、このRコードを生成するPerlのプログラムを作成してからそれをRで実行していました。現在よく使われているGSEA(Gene Set Enrichment Analysis)のハシリで、遺伝子にアノテーションされたGene Ontologyの高次(根っこに近い)タームで集計してその分布を見ていました</p>
</div>

<h3 id="content_1_2"><a id="z73b23bc" href="" title="z73b23bc"><span class="sanchor">_</span></a> 2. 階層的クラスタリング  </h3>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>【実習2】Rを起動し以下のファイルに記述されたRのコマンド(<strong>02hclust.r</strong>)を実行しなさい。データファイルとして必要な<strong>matrix.txt</strong>をgithubのサイトから<a href="https://raw.githubusercontent.com/bonohu/AJACS51/master/matrix.txt" rel="nofollow">ダウンロード</a>し、現在作業しているディレクトリにおいておく必要があります。実行後、そのディレクトリに<strong>hclust.png</strong>というファイルが新たに生成されていることを確認しなさい。</li></ul>
<pre>png(&quot;hclust.png&quot;)
d &lt;- read.table('matrix.txt')
c &lt;- hclust(as.dist(d), method = 'average')
plot(c, hang=-1)
dev.off()</pre>
<a name="plugin_fold_anchor5"></a>
<div class="plugin_fold_title_plus" onclick="return plugin_fold_onclick(this,event,'plugin_fold_anchor5')"><p>←左の+マークをクリックするとこのスクリプトの経緯が…</p>
</div>
<div class="plugin_fold_body"><p>このプログラムはかつての蛋白質・核酸・酵素に寄稿したレビューで紹介したもので、当時階層的クラスタリングをフリーウェアで実行する手段として重宝された(はず)。<a href="http://www.ncbi.nlm.nih.gov/pubmed/12638180" rel="nofollow">Bono H and Nakao MC, PNE 2003</a></p>
</div>

<h3 id="content_1_3"><a id="uca2d586" href="" title="uca2d586"><span class="sanchor">_</span></a> 3. Affymetrixデータ正規化  </h3>
<p>このパートは以下のブログを参考に。 <a href="http://bonohu.jp/blog/2013/06/17/justrma/" rel="nofollow">http://bonohu.jp/blog/2013/06/17/justrma/</a></p>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>【実習3】Bioconductorを利用しましょう。Rを起動して以下のコマンドでaffy libraryをインストールしなさい。
<pre>source(&quot;http://bioconductor.org/biocLite.R&quot;)
biocLite(&quot;affy&quot;)</pre>
そして、affy library中のjustRMA関数を利用して、<a href="http://allie.dbcls.jp/short/exact/Any/RMA.html" rel="nofollow">RMA</a>(Robust Multichip Average)正規化してみましょう。自らのAffymetrix Genechip データ(CELファイル)がある人はそれを、ない人は<a href="http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE17264" rel="nofollow">GSE17264 Comparative transcriptome analysis of dedifferentiation in porcine mature adipocytes and follicular granulosa cells</a>にある生データ(CELファイル)で実行しましょう。実行する際、Session → Set Working Directory を、CELファイルをダウンロードしてきたディレクトリに指定することがポイントです。それができたら、以下のコード(<strong>03justrma.r</strong>)を実行します。
<pre>source(&quot;http://bioconductor.org/biocLite.R&quot;)
biocLite(&quot;affy&quot;)
library(affy)
write.exprs(justRMA(), file=&quot;RMA.txt&quot;)</pre>
その結果生成される<strong>RMA.txt</strong>ファイルがRMAによって正規化された遺伝子発現データになります。</li></ul>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>【発展2】上記の実習のサンプルデータは <a href="http://www.ncbi.nlm.nih.gov/gds?term=GPL3533%5BAccession%5D&amp;cmd=search" rel="nofollow">GPL3533 [Porcine] Affymetrix Porcine Genome Array</a> と呼ばれる(カタログ)マイクロアレイ(プラットフォーム)を使ったデータでした。同じマイクロアレイ(プラットフォーム)のデータであればjustRMAを実行することが出来ます。同じプラットフォームのデータの中からiPS細胞のものを探しだして上記のデータに混ぜてjustRMAを実行しなさい。
<a name="plugin_fold_anchor6"></a>
<div class="plugin_fold_title_plus" onclick="return plugin_fold_onclick(this,event,'plugin_fold_anchor6')"><p>←こたえ(の一つ)は左の+マークをクリックすると出てきます</p>
</div>
<div class="plugin_fold_body"><p>GSE15472 Induced Pluripotent Stem Cells from the Pig Somatic Cells の3つのサンプル(GSM388154,GSM388155,GSM388156)がそれです<a href="http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE15472" rel="nofollow">http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE15472</a> CELファイルをダウンロードして同じdirectoryに置き、再度justRMAを実行してみましょう</p>
</div></li></ul>
<a name="plugin_fold_anchor7"></a>
<div class="plugin_fold_title_plus" onclick="return plugin_fold_onclick(this,event,'plugin_fold_anchor7')"><p>←左の+マークをクリックするとこの節の経緯が...</p>
</div>
<div class="plugin_fold_body"><p>このマイクロアレイデータは以下の論文で我々が発表したもので、全く同じ手段で正規化を行いました。また、発展課題にある公共遺伝子発現データを足したデータ解釈はやはりこの論文で実際に発表したもので、我々のマイクロアレイデータの生物学的解釈(DFATがMA(Mature AdipocyteよりもiPSに近い)に役だっています <a href="http://www.ncbi.nlm.nih.gov/pubmed/21419102" rel="nofollow">Ono H, Oki Y, Bono H, Kano K. BBRC 2011</a></p>
</div>

<h3 id="content_1_4"><a id="od88c1b7" href="" title="od88c1b7"><span class="sanchor">_</span></a> 4. 主成分分析(PCA)  </h3>
<p><a href="http://allie.dbcls.jp/short/exact/Any/PCA.html" rel="nofollow">PCA</a>(Primary Component Analysis)。
このパートは以下のブログを参考に。<a href="http://bonohu.jp/blog/2013/07/13/pcaonr/" rel="nofollow">http://bonohu.jp/blog/2013/07/13/pcaonr/</a></p>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>【実習4】Rを起動し以下のファイルに記述されたRのコマンド(04pca.r)を実行しなさい。データファイルとして必要なRMA.txtは実習3で作成したものを利用し、現在作業しているディレクトリにおいておく必要があります(うまく行かなかった人はUSBメモリでデータをお渡しします)。
<pre>data &lt;- read.table(&quot;RMA.txt&quot;, header=TRUE, row.names=1, sep=&quot;\t&quot;, quote=&quot;&quot;) 
data.pca &lt;- prcomp(t(data))
names(data.pca)
plot(data.pca$sdev, type=&quot;h&quot;, main=&quot;PCA s.d.&quot;)
data.pca.sample &lt;- t(data) %*% data.pca$rotation[,1:2]
plot(data.pca.sample, main=&quot;PCA&quot;)  
text(data.pca.sample, colnames(data), col = c(rep(&quot;red&quot;, 3), rep(&quot;blue&quot;,3),rep(&quot;green&quot;,3),rep(&quot;black&quot;,3)))</pre></li></ul>
<p>図に<strong>.CEL.gz</strong>の文字がいっぱい入っていて見づらい?そう思った方はこちらを参照 <a href="http://bonohu.jp/blog/2014/09/10/afterjustrma/" rel="nofollow">http://bonohu.jp/blog/2014/09/10/afterjustrma/</a></p>

<h3 id="content_1_5"><a id="xbad7386" href="" title="xbad7386"><span class="sanchor">_</span></a> 5. NGS解析(cuffdiffの結果可視化)  </h3>
<p>cufflinksパッケージにcuffdiffという発現データの差分を計算するプログラムがあります。それを実行したあとのデータを可視化する手段として使うBioconductorのパッケージにcummeRbundがあります(<a href="http://dx.doi.org/10.1371%2Fjournal.pone.0104283" rel="nofollow">cuffdiffの使用例</a>)。</p>
<p>参考：<a href="http://connpass.com/event/8978/" rel="nofollow">Mishima.syk#4</a>の<a href="http://figshare.com/articles/NGS_RNAseq_/1216717" rel="nofollow">bonohuの発表資料「NGS解析(RNAseq)」</a></p>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>【発展3】Rを起動し以下のファイルに記述されたRのコマンドを実行しなさい。データファイルとして必要なcuffdiffディレクトリ以下のファイルは手持ちのものか、なければサンプルをUSBメモリでお渡しします。現在作業しているディレクトリにおいておく必要があります。
<pre>source(&quot;http://bioconductor.org/biocLite.R&quot;)
biocLite(&quot;cummeRbund&quot;)
library(&quot;cummeRbund&quot;)
cuff.dir &lt;- &quot;cuffdiff&quot;
cuff &lt;- readCufflinks(dir=cuff.dir)</pre>
準備はここまでで、以下のコマンドで発現密度分布のプロット
<pre>dens &lt;- csDensity(genes(cuff))
dens</pre>
CSV形式でFPKM値を出力などができます。
<pre>gene.matrix &lt;- fpkmMatrix(genes(cuff))
write.csv(gene.matrix, file=&quot;fpkm.csv&quot;)</pre></li></ul>

<h3 id="content_1_6"><a id="sc4355c8" href="" title="sc4355c8"><span class="sanchor">_</span></a> 6.最後に: R Graphical Manualのすゝめ  </h3>
<p>どんなライブラリがあって、どういった可視化ができるかは<a href="http://rgm3.lab.nig.ac.jp/RGM" rel="nofollow">R Graphical Manual</a>が大変便利です。</p>
<ul class="list1" style="padding-left:16px;margin-left:16px"><li>【実習5】ウェブブラウザを起動し、インターネット検索で<strong>R Graphical Manual</strong>を検索してサイトに辿り着き、サイト内の検索フォームからキーワード<strong>cummeRbund</strong>で検索しなさい。どのような結果が返ってきたか?前節で紹介したcsDensityのエントリを見つけてみなさい。</li>
<li>【発展4】csDensityのエントリにあるサンプルコードを参考に、<strong>csVolcano</strong>や<strong>dispersionPlot</strong>などのエントリ中のサンプルコードを現在の例に合うように改変して実行してみなさい。</li></ul>
