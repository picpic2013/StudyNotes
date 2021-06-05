## 基础语法

### 模板

一篇tex文章的一般格式如下

~~~tex
\documentclass[UTF8,10pt]{ctexart}

% 头部部分，用于定义格式（类似CSS）、引用的脚本（类似 Python 的包）等

% 常用的脚本
\usepackage{graphicx}
\usepackage{makecell}
\usepackage{geometry}
\usepackage{xeCJK}

% 为交叉引用定义命令
\newcommand{\upcite}[1]{\textsuperscript{\textsuperscript{\cite{#1}}}}

% 设置格式
\CTEXsetup[format={\Large\bfseries}]{section}

% 设置页边距
\geometry{left=3.17cm,right=3.17cm,top=2.54cm,bottom=2.54cm}

% 设置默认字体
\setmainfont{Times New Roman}
\setCJKsansfont {FangSong}
% 设置正文无衬线族的CJK字体，影响\sffamily和\textsf 的字体
\setCJKmonofont {FangSong}
% 设置正文等宽族的CJK字体，影响\ttfamily 和 \texttt 的字体

% 设置页面风格模板
\pagestyle{plain}

% 设置标题和日期
\title{\fontsize{50pt}{0} 音视频内容数字版权管理区块链应用需求白皮书}  %文章标题
\date{\today}  


\begin{document}
% 正文内容，类似 HTML 的 body 部分
\end{document}
~~~



### 注释

~~~tex
% 这是一个注释
~~~



### 正文格式

直接写即可



### 标题格式

~~~tex
% 一级标题
\section{一级标题}

% 二级标题
\subsection{二级标题}

% 三级标题
\subsubsection{三级标题}
~~~



### 图片格式

~~~tex
% 一个例子
\begin{figure}
    \centering
    % 指定图片大小 & 图片路径
    \includegraphics[width=0.5\textwidth]{img/img-xl-1.png}
    % 指定图片名称
    \caption{DCI登记办理流程}
    % 引用时输入的名称
    \label{某某人的图1}
\end{figure}


% 正文引用时
其中DCI登记办理流程如图\ref{某某人的图1}所示。
~~~



### 表格格式

~~~tex
\begin{table}
\centering
\caption{我国数字版权相关文件}
\begin{tabular}{|l|l|l|}
\hline
\makecell[c]{时间} & \makecell[c]{名称} & \makecell[c]{相关规定} \\ \hline

2015年7月 & \makecell[l]{《关于积极推进“互联 \\ 网+” 行动的指导意见》} & \makecell[l]{加强数字版权和专利执法工作，严厉打击各种网络侵权 \\ 假冒行为。} \\ \hline

2015年7月 & \makecell[l]{《关于责令网络音乐服务 \\ 商停止授权传播音乐 \\ 作品的通知》} & \makecell[l]{要求各网络音乐服务商于7月31日前将未经授权传播 \\ 的音乐作品全部下线。} \\ \hline

2015年11月 & \makecell[l]{《关于加强互联网领域 \\ 侵权假冒行为治理的意见》} & \makecell[l]{强化对网络（手机）文学、音乐、影视、游戏、动漫、 \\ 软件等重点领域的监测监管，及时发现和查处网络 \\ 非法转载等各类侵权盗版行为。} \\ \hline

2016年6月 & \makecell[l]{《移动互联网应用程序信息 \\ 服务管理规定》} & \makecell[l]{明确要求“移动互联网应用程序提供者”和“互联网 \\ 应用商店服务提供者”应依法严格履行相关义务和管理 \\ 责任，尊重和保护知识产权。} \\ \hline

2016年11月 & \makecell[l]{《电影产业促进法》} & \makecell[l]{明确规定电影相关的知识产权受法律保护，同时对涉及 \\ 信息网络传播的公映电影进行规范。} \\ \hline

2017年6月 & \makecell[l]{《深入实施国家知识产权 \\ 战略加快建设知识产权 \\ 强国推进计划》} & \makecell[l]{提出加强网络侵权盗版治理、探索对新型数字版权侵权 \\ 盗版行为的有效治理模式等具体工作措施。} \\ \hline

2017年6月 & \makecell[l]{《关于规范电子版作品 \\ 登记证书的通知》} & \makecell[l]{针对电子作品登记证书的出具、制发过程存在的问题 \\ 提出要求。} \\ \hline

2018年12月 & \makecell[l]{《关于审查知识产权 \\ 纠纷行为保全案件适用 \\ 法律若干问题的规定》} & \makecell[l]{将“时效性较强的热播节目正在或者即将受到侵害” \\ 列为“情况紧急”情形之一，保障了热播节目的信息 \\ 网络传播权。} \\ \hline

2019年2月 & \makecell[l]{《粤港澳大湾区发展 \\ 规划纲要》} & \makecell[l]{提出强化数字版权保护和运用，不断丰富、发展和完善 \\ 有利于激励创新的保护制度。} \\ \hline

2019年10月 & \makecell[l]{《优化营商环境条例》} & \makecell[l]{明确规定严厉打击数字版权的侵犯违法行为，建立健全 \\ 数字版权惩罚性赔偿制度，提升审查质量及效率。} \\ \hline

\end{tabular}
\label{柴表1}
\end{table}
~~~



### 交叉引用

~~~tex
% 正文里使用在头上定义的命令 \upcite 来自动生成引用
\upcite{自己给引用起一个名字}

% 指定参考文献块 & 最多 99 条参考文献。这个 99 可以修改
\begin{thebibliography}{99} 
	% 给每个参考文献都起一个名字
	\bibitem{自己给引用起一个名字}胡萌.区...
\end{thebibliography}
~~~

