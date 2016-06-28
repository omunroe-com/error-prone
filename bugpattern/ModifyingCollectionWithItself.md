---
title: ModifyingCollectionWithItself
summary: Modifying a collection with itself
layout: bugpattern
category: JDK
severity: ERROR
maturity: EXPERIMENTAL
---

<!--
*** AUTO-GENERATED, DO NOT MODIFY ***
To make changes, edit the @BugPattern annotation or the explanation in docs/bugpattern.
-->

## The problem
Modifying a collection with itself is almost never what is intended. collection.addAll(collection) and collection.retainAll(collection) are both no-ops, and collection.removeAll(collection) is equivalent to collection.clear().

## Suppression
Suppress false positives by adding an `@SuppressWarnings("ModifyingCollectionWithItself")` annotation to the enclosing element.

----------

### Positive examples
__ModifyingCollectionWithItselfPositiveCases.java__

{% highlight java %}
/*
 * Copyright 2011 Google Inc. All Rights Reserved.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.google.errorprone.bugpatterns.testdata;

import java.util.ArrayList;
import java.util.List;

/**
 * @author scottjohnson@google.com (Scott Johnson)
 */
public class ModifyingCollectionWithItselfPositiveCases {
  
  List<Integer> a = new ArrayList<Integer>();
  List<Integer> c = new ArrayList<Integer>();
  
  public boolean addAll(List<Integer> b) {
    // BUG: Diagnostic contains: a.addAll(b)
    return this.a.addAll(a);
  }
  
  public boolean removeAll(List<Integer> b) {
    // BUG: Diagnostic contains: a.containsAll(b)
    return this.a.containsAll(this.a);
  }
  
  public boolean retainAll(List<Integer> a) {
    // BUG: Diagnostic contains: this.a.retainAll(a)
    return a.retainAll(a);
  }
  
  public boolean containsAll() {
    // BUG: Diagnostic contains: a.clear()
    return this.a.removeAll(a);
  }
}
{% endhighlight %}

### Negative examples
__ModifyingCollectionWithItselfNegativeCases.java__

{% highlight java %}
/*
 * Copyright 2011 Google Inc. All Rights Reserved.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.google.errorprone.bugpatterns.testdata;

import java.util.ArrayList;
import java.util.List;

/**
 * @author scottjohnson@google.com (Scott Johnson)
 */
public class ModifyingCollectionWithItselfNegativeCases {
  
  List<Integer> a = new ArrayList<Integer>();
  
  public boolean addAll(List<Integer> b) {
    return a.addAll(b);
  }
  
  public boolean removeAll(List<Integer> b) {
    return a.removeAll(b);
  }
  
  public boolean retainAll(List<Integer> b) {
    return a.retainAll(b);
  }
  
  public boolean containsAll(List<Integer> b) {
    return a.containsAll(b);
  }
}
{% endhighlight %}
