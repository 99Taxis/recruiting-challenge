# Feel like joining the Yellow Side?

We are glad to hear that! But, you known, as Linus memorably put it,
“Talk is cheap. Show me the code.” So fire up your favorite editor and
remember that unearthly nugget of wisdom: “Do or do not. There is no try.”

## Route Simplification

### Motivation

We would like to reduce the number of space-time points with which our routes
are represented, not only to save storage space, but also to make route processing
algorithms (e.g., circuits detection) more efficient.

General lossless compression algorithms (e.g., gzip) modify the file format and do
not reduce the number of space-time points, so although they may reduce storage
requirements they do not make route processing algorithms more efficient (on the
contrary, they demand decompression before any route processing). Therefore, we
chose to develop a specialized solution.

### Requirements

In this challenge you will implement a lossy compression algorithm which
eliminates redundant points in a route.

A route is defined as an ordered sequence of time-space points.
A space-time point is defined as a pair `(t, s)`, where `s` is the point
on the surface of the Earth where the vehicle were at time instant `t`.
The geographical point `s` is specified by its latitude and longitude.

A point is considered redundant if it can be predicted, within a prespecified
error margin, from its predecessors by means of linear extrapolation, such as
when the vehicle is parked or driving in a straight line with constant speed.
The following linear extrapolation scheme is to be adopted:
`predicted_location(y[i], v, dt) = y[i] + v * dt`, where `y[i]` is the
predecessor to the point `x[j]` being predicted, `v` is the vectorial speed
estimate at `y[i]` and `dt` is the time difference between `x[j]` and
`y[i]`. Points `x[.]` are from the original input route and points `y[.]`
pertain to the simplified output route. Speed is assumed to be zero at the
beginning of the route. The last time-space point of the original route must
_always_ be included in the simplified one.

Defining U as the original route and V as the simplified one, the data in V
should suffice to predict all points in U within the allowed error margin
by means of the aforementioned prediction scheme. Therefore, given V and
the time instants from U, one should be able to, using the aforesaid
extrapolation method, reconstruct the original V within the allowed error
margin. Bear in mind that you are _not_ required to implement the
reconstructor / decompressor in this challenge.

#### Tolerance

The maximum allowed extrapolation error at any time instant from the original
route is 10 meters. Extrapolation error is defined as the [orthodomic distance](https://en.wikipedia.org/wiki/Great-circle_distance) between the original and extrapolated geographical points.

#### Complexity

Your solution should ideally satisfy the following complexity constraints:
**S = O(1)** and **T = O(n)**. Therefore, the memory usage of your
solution should not depend on the input size and the time it requires
to run should be directly proportional to the input size.

The complexity constraints require a greedy approach which is not
guaranteed to always achieve maximum compression level. That is ok.

#### I/O Format

Input: string in [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) format, with chronologically sorted sequence of `timestamp,longitude,latitude` space-time points.

Output: same as input, without the redundant points. Caveat: the output points should
have exactly the same accuracy and precision and contain no unnecessary characters,
such as trailing zeroes or whitespace.

##### Example

###### Input

```
100,-43.00495,-22.82115
104,-43.00495,-22.82115
114,-43.00495,-22.82115
120,-43.00495,-22.82115
124,-43.00495,-22.82115
126,-43.00537,-22.82112
129,-43.00537,-22.82112
137,-43.00537,-22.82112
152,-43.00480,-22.82112
166,-43.00480,-22.82112
187,-43.00480,-22.82112
197,-43.00480,-22.82112
207,-43.00480,-22.82112
220,-43.00480,-22.82112
230,-43.00480,-22.82112
233,-43.00518,-22.82115
248,-43.00450,-22.82010
253,-43.00541,-22.82115
258,-43.00480,-22.82112
265,-43.00480,-22.82112
276,-43.00480,-22.82112
284,-43.00480,-22.82112
```

###### Output

```
100,-43.00495,-22.82115
126,-43.00537,-22.82112
137,-43.00537,-22.82112
152,-43.0048,-22.82112
166,-43.0048,-22.82112
233,-43.00518,-22.82115
248,-43.0045,-22.8201
253,-43.00541,-22.82115
258,-43.0048,-22.82112
265,-43.0048,-22.82112
284,-43.0048,-22.82112
```

#### Documentation

Please document your assumptions and present the reasoning behind them.

### Submission Instructions

Please send your solution via email to [coding-challenge@99taxis.com](mailto:coding-challenge@99taxis.com),
with your source code file(s) and README attached. Please use one of the
following programming languages: Scala, Java, Python or Ruby.

### Final remarks

Help keep things interesting and fair and please do not upload your
solution to any public place on the Internet. “Once you start down
the dark path, forever will it dominate your destiny.”
