---
title: General Differences between a WebGL and a Native 3D Application
active_menu: learn
---

The biggest difference between a WebGL and a native 3D application is in the way the data is loaded. A native 3D application loads its data from the hard drive. Hard drives have become so fast they can transfer up to **500 MB per second**. Overhead for accessing individual files goes towards zero. But for a WebGL application the data typically comes from a server over the Internet. This usually means around **1 MB per second** for broadband and DSL connections and each individual file has some HTTP header and sometimes even reconnection **overhead**. If HTTPS is used **data encryption** comes into the mix as well costing additional load time.

So while it is possible to make a 500 MB WebGL application with hundreds of individual files and load them all into memory and the graphics card it is **not what is recommended for a great user experience with WebGL** because it will take minutes to load and might take forever on mobile devices. **If you come from a desktop application background you have to radically change the way you think about writing applications for WebGL.**

Here is a little table comparing access to performance relevant resources of a desktop 3D application to a WebGL application:

<table border="1" width="100%" style="margin-bottom: 20px">
  <tr>
    <th></th>
    <th>Desktop 3D application</th>
    <th>WebGL application</th>
  </tr>
  <tr>
    <td>Data access</td>
    <td bgcolor="lightgreen">hard drives</td>
    <td>Internet server</td>
  </tr>
  <tr>
    <td>CPU access</td>
    <td bgcolor="lightgreen">multithreaded native code </td>
    <td>just-in-time compiled JavaScript</td>
  </tr>
  <tr>
    <td>Physics</td>
    <td bgcolor="lightgreen">hardware accelerated or native</td>
    <td>just-in-time compiled JavaScript</td>
  </tr>
  <tr>
    <td>Memory</td>
    <td bgcolor="lightgreen">direct and fast</td>
    <td bgcolor="lightgreen">indirect but still fast, garbage collected</td>
  </tr>
  <tr>
    <td>GPU access</td>
    <td bgcolor="lightgreen">geometry, fragment and vertex shader</td>
    <td bgcolor="lightgreen">fragment and vertex shader access</td>
  </tr>
</table>


As you can see the only performant things a WebGL and a desktop 3D application have in common are the GPU and memory access. So there lies the secret to performant WebGL applications: **Limit the data needed to be transferred, make use of the local memory and offload complex calculations to the GPU.**

The [computer demo scene](http://en.wikipedia.org/wiki/Demoscene) can be a great source of inspiration for this kind of thinking. The scene has created amazing content with limited resources for decades. Another great source of inspiration of what can be done just with shaders alone are the great examples at [shadertoy.com](https://www.shadertoy.com/). 

**It is all about knowing your resources and using them to their best potential.**

We will go over many ways to accomplish this goal in detail in this article series.

## Mobile-First Development

A very important development approach for WebGL applications which also **need to run on mobile platforms** is to test them as soon as possible on the target devices. In fact, they should be the primary test platform while developing, because they have **much lower limits** on many aspects of the resources listed above. First of all, there is a much smaller pool of memory. Second, the CPU and GPU and less powerful. Third, WebGL allows for a lot of leeway on the implementation limits of certain parameters, for example the "**Max Texture Size**" or the “**Max Vertex Uniform Vectors**” ([Click here for your numbers](http://www.browserleaks.com/webgl)).

So if it runs good on mobile devices it will run great on desktops as well. The reverse does not necessarily hold true. While it is not impossible to "port" a WebGL application to mobile devices, it is much less work if you develop and test on mobile devices right from the start.

Next: [Reducing Code Size](../reducing-code-size/)

