---
layout: post
title:  "Calling APIs"
date:   2016-09-12 11:26:48 +0100
categories: API-Clients
---
This document covers the standard procedure for calling HTTP services and APIs

# Using HttpClient

When using HttpClient it is important NOT to use it wrapped in a ```using``` block; this causes a new connection to be opened on the remote server for every request. See [this post](http://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/) for more information.

It is preferable to set up an HttpClient as a singleton or instansiate one in the constructor of your class and inject youe class as a singleton.
{% highlight c# %}
public class SomeApiClient:ICallSomeApi{
  HttpClient _httpClient;
  public SomeApiClient(Uri someUri){
    _httpClient = new HttpClient(someUri);
  }

  //some methods that call someApi...
}
{% endhighlight %}

# Using 3rd party HttpClients

There are several 3rd party HttpClients such as RestSharp and FluentHttpClient. These are very good but it is preferable not to take a dependancy on them unless it is clear that using them over HttpClient and HttpClient extensions will speed up development.