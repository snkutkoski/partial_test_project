This project contains a simple Rails application that I use to perform benchmarking on partial rendering.

The results obtained so far are outlined and described below. This benchmarking was done because I suspect that rendering partials in Rails is relativley slow. My experiments were based on [this](http://www.justinweiss.com/articles/how-much-time-does-rendering-a-partial-really-take/) blog post I read and wanted to verify. If the cost of a partial is high engough, I hope to create a "simpler partial" gem that bypasses a lot of the overhead that ActionView seems to have in its partial rendering but that still contains the most important features of partials, like local variables and access to helpers.

|                                 | user     | system   | total      | real     |
|---------------------------------|----------|----------|------------|----------|
| inline                          | 0.010000 | 0.000000 | 0.010000   | 0.004263 |
| partial                         | 1.480000 | 0.130000 | 1.610000   | 1.619102 |
| collection_partial_using_loop   | 1.590000 | 0.120000 | 1.710000   | 1.996031 |
| collection_partial_using_option | 0.160000 | 0.000000 | 0.160000   | 0.156621 |

The above numbers indicate the number of seconds it took to render 10,000 p HTML elements for a variety of partial rendering strategies. They were run on ruby 2.2.3 and rails 4.2.6 in production mode. I wouldn't call these results perfect (they were run on my laptop with lots of other applications running at the same time), but they do seem to indicate a clear pattern.

Rendering 10,000 p elements inline is almost instant. However, rendering them using a partial takes about 0.16 ms longer per p element. Using the collection option on render was also nearly instant.

I was actually expecting partial rendering to be slower than this. And the collection option could also speed up many views. However, I beleve there could still be use cases for a faster partial. For example, if you are already rendering partials with the colleciton option, and those partials each render some of their own partials, then you will run into the n-partials rendered problem. After the "simple partial" gem is done, I will return to this project to benchmark it against these the default Rails partials.
