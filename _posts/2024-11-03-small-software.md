---
layout: post
title:  "Small Software for Small Data"
date:   2024-11-03 21:10:00 +0100
category: software
feature: true
---

In this post, I'd like to reflect on the reasons why I think there's a place for small software tailored for small data and my motivation for building a data visualisation tool called _Visprex_.

<h2>Why Small Data?</h2>

In February 2023, a blog article titled [Big Data is Dead] was published by MotherDuck [^1], a cloud data warehouse company built on top of [DuckDB]. Among several reasons why the prophesied Big Data movement is often misguided, one explanation that resonated with me the most is the separation between the analytics workload and the overall data sizes.

Reflecting on his time working on Google's BigQuery, the author Jordan Tigani writes:

> Customers with giant data sizes almost never queried huge amounts of data

which is also reflective of my experience working at a large marketplace company with hundreds of millions of monthly transactions, where key analytics decisions were made from representative samples by data analysts and domain experts.

Another distinction I would like to illustrate here is that the type and size of data required in analytics and modelling are somewhat different from one another.

In the modern machine learning context, modelling is often associated with non-parametric models (i.e. neural networks) which benefit from having more training data. More concretely, it helps to train the model with edge case scenarios which minimises the loss function on top of capturing general data trends, as the model is able to run inference on specific data points using high-dimensional features.

On the other hand, the output of analytics work is mainly actionable insights for business functions, where the stakeholders' primary interest is coming up with a policy or a campaign that is effective for the average user in some segment or cohort. For those purposes, having representative user data is often more than enough.

There are always nuances in how analytics workload should be handled, but I do agree with the main points of the article that it is time to acknowledge the headaches associated with infrastructures built for scale, and that there's room for small software in some contexts such as analytics and parametric modelling where a simple, single-machine software can solve most of your problems at hand.

<h2>Building Small Software</h2>

In the spirit of building small software, I have been working on a side project called [Visprex] for over a year now, which enables you to use your browser as a data visualisation tool for your CSV files.

The main features of Visprex include the ability to plot histograms, scatterplots, and a correlation matrix, with additional utility tools for inspecting individual data points from plots, feature transformations, and filtering. See [Visprex Docs] for more details and use cases.

<h3>Motivation and Architectural Choices</h3>

The focus on these visualisation steps is motivated by my background in economics which I read for my undergraduate degree. I spent a lot of time thinking about data distributions and linear relationships between features during this time, and I believe a small tool like Visprex can help you quickly build an intuition about your dataset without referring to data visualisation libraries on your software, which in my opinion is the least creative part of data science workflows.

While this software is not suitable for large datasets (>100 MB), it is sufficient to get an idea of data distributions from representative samples in many analytics cases.

Another choice I made for this small data use case is to <b>process everything on the frontend</b> with TypeScript. While this naturally limits the capacity to handle large data, it provides two important benefits for the potential user and the maintainer.

The benefit for the user is data privacy. You can load sentivie data and don't have to worry about it leaving your laptop. Convincing the user of this, however, is another story. A technical person can check the Network tab to see that there are no network calls made. Another simpler way to ensure this is to turn off your Wi-Fi after the site loads. While some mobile applications are successful at this, communicating that your website works offline remains a difficult problem.

The benefit for the maintainer is low upkeep. It costs very little for me to run this project as everything is bundled into less than 50 kilobytes of static assets and a few hundred kilobytes of minified JavaScript code, and I do not have to worry about maintaining a database which tends to be costly even for a small project.

<h3>Upcoming Features</h3>

I'm planning on adding a few more features to Visprex, and one is visualisation for time series data. I'm using [Papa Parse] for parsing data from CSV files, and I need to come up with a better way to handle timestamps of different formats to conditionally display line charts for different features on the Y axis and the timestamp on the X axis.

Another feature I'm keen to work on is [DuckDB Wasm] integration, which would add a SQL editor tab for transforming and querying data and then updates the in-memory data automatically as inputs for your visualisation. While this increases the bundle size and is useful for advanced users who can write SQL queries in practice, it addresses the main limitation of _everything-on-the-frontend_ approach, as WebAssembly can handle up to 4 GB of in-memory data [^2].

<h3>Looking Back</h3>

It's been an enjoyable and fulfilling experience so far to be able to build and showcase a tool that I personally think is useful to my friends and colleagues without worrying about data privacy or running costs. As my day jobs have always been in backend systems, I learnt a lot about building frontend applications with TypeScript and React along with managing a domain and writing documentation.

I'd also like to give credit to Dr Sean Brocklebank from the University of Edinburgh and Dr Anna Brocklebank for providing feedback on the early iterations of Visprex.

If you have any feedback or feature requests, please feel free to reach out at [kengo@hey.com]. If you're interested in how I built Visprex, the code is open-source on [GitHub].

[Visprex]: https://visprex.com
[Visprex Docs]: https://docs.visprex.com
[GitHub]: https://github.com/visprex/visprex
[Big Data is Dead]: https://motherduck.com/blog/big-data-is-dead/
[DuckDB]: https://duckdb.org/
[Papa Parse]: https://www.papaparse.com/docs
[DuckDB Wasm]: https://duckdb.org/docs/api/wasm/overview.html
[kengo@hey.com]: mailto:kengo@hey.com

----

[^1]: [Hacker News discussion](https://news.ycombinator.com/item?id=34694926) on Big Data is Dead
[^2]: [StackOverflow](https://stackoverflow.com/questions/40417774/memory-limits-in-webassembly) post on memory limits in WebAssembly