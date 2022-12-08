---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS
css: unocss
---

# SOME CODING BEST PRACTICES

---

# WHAT MAKES A CLEAN CODE?

<div v-click style="margin-bottom: 10px">1. Easy to read</div>
<div v-click style="margin-bottom: 10px">2. Easy to improve</div>
<div v-click style="margin-bottom: 10px">3. Easy to fix</div>
<div v-click style="margin-bottom: 10px">4. Easy to test</div>
<div v-click>5. Easy to navigate around</div>

---

# 1. An organised and consistent import structure

Framework specific imports, third party libraries and internal app modules
should be arranged in an organised manner.

<div style="display:flex; justify-content: space-between">
<div style="margin-right: 10px" v-click>
  <div>BEFORE</div>

  ```ts
    import React, { useContext, useState } from "react";
    import CategoryBadge from "./CategoryBadge";
    import { 
      Box, Text, Button, Flex, Avatar 
    } from "@chakra-ui/react";
    import AppDialog from "./AppDialog";
    import { AiFillLike } from "react-icons/ai";
    import useReadingList from "../hooks/useReadingList";
    import { BiLike } from "react-icons/bi";
    import { resourceLikesContext } from "../context/resource";
    import useFetchNotes from "../hooks/useFetchNotes";
    import { ADD_TO_READING_LIST } from "../models/readingList";
    import { Resource as ResourceType } from "../types";
    import { auth } from "../main";
  ```
</div>

<div v-click>
  <div>AFTER</div>

  ```ts
    import React, { useContext, useState } from "react";

    import { 
      Box, Text, Button, Flex, Avatar,
    } from "@chakra-ui/react";
    import { BiLike } from "react-icons/bi";
    import { AiFillLike } from "react-icons/ai";

    import CategoryBadge from "./CategoryBadge";
    import AppDialog from "./AppDialog";
    import useFetchNotes from "../hooks/useFetchNotes";
    import useReadingList from "../hooks/useReadingList";
    import { resourceLikesContext } from "../context/resource";
    import { ADD_TO_READING_LIST } from "../models/readingList";
    import { Resource as ResourceType } from "../types";
    import { auth } from "../main";
  ```
</div>
</div>

---

# 2. Write declarative code

  Declarative programming is when you write your code in such a way that it 
  <em style='color: lightgreen'>describes</em> what you want to do, and not how you want to do it

<div style="display: grid; grid-template-columns: 1fr 1fr; column-gap: 10px; margin-top: 10px">
  <div v-click>
  <div>BEFORE</div>

  ```ts
    if(
       !!objectives
      .find(
        objective => objective?.keyResults?
        .find(keyResult => keyResult?.reviewwer_id)
      )
    ) {
      // do something
    }
  ```
  </div>

  <div>
  <div v-click>
  <div>AFTER</div>

  ```ts
    if(objectiveHasReviewer()) {
      // do something
    }
  ```

  </div>
  <div v-click>

  ```ts
    const objectiveHasReviewer = () => !!objectives.find(kpiHasReviewer);

    const kpiHasReviewer = (objective) => {
      return objective?.keyResults?.find(keyResult => keyResult.reviewer_id)
    }
  ```

  </div>
  </div>
</div>

---

# 3. Avoid mixed levels of abstraction

The levels of abstraction inside a function should not be mixed

<div>
<div v-click>
<div>BEFORE</div>

```ts
  const addToReadingList = async (resourceId) => {
    const item = readingList.find(item => item.resource_id === resourceId);

    if (item) return;
    await READING_LIST_MODEL.addToReadingList(resourceId)
  }
```

</div>

<div style="margin-top: 20px" v-click>
<div>AFTER</div>

```ts

  const addToReadingList = async (resourceId) => {
    if (isInReadingList(resourceId)) return;

    await READING_LIST_MODEL.addToReadingList(resourceId)
  }
```

</div>

<div v-click>

```ts
  // predicate functions
  const isInReadingList = (resourceId) => !!readingList.find((item) => resourceIdMatch(item, resourceId));
  const resourceIdMatch = (item, resourceId) => item.resource_id === resourceId;
```

</div>

</div>


---

# 4. Avoid more than 3 function parameters
Too many function params can lead to bugs.
Instead, consider using an object.

<div v-click>
<div>BEFORE</div>

```ts
  const test = (age, location, gender, maritalStatus) => {
    // do stuff
  }
```
</div>

<div style="margin-top: 20px" v-click>
<div>AFTER</div>

```ts

  const test = (userDetails) => {
    const { age, location, gender, maritalStatus } = userDetails;

    // do stuff
  }
```
</div>

---

# 5. A cleaner control flow (*Debatable*)

You can avoid too many else if statements by using<br>
the strategy pattern (my favorite)

<div style="display: grid; grid-template-columns: repeat(2, 1fr); column-gap: 20px">
<div v-click>
  BEFORE

  ```ts
    const test = (accountType) => {
      if (accountType === 'savings') {
        // do something
      } else if (accountType === 'current') {
        // do something
      } else if (accountType === 'fixed') {
        // do something
      } else if (accountType === 'merchant') {
        // do something
      } else if (accountType === 'flexy') {
        // do something
      } else if (accountType === 'seniors') {
        // do something
      }
    }
  ```
</div>

<div v-click>
AFTER

```ts
  const accountStrategy = {
    savings: savingsFn,
    current: currentFn,
    fixed: fixedFn,
    merchant: merchantFn,
    flexy: flexyFn,
    seniors: seniorsFn
  }

  const test = (accountType) => {
    accountStrategy[accountType]();
  }
```

</div>

</div>


---

# Quick reminder of some common best practices

<div v-click style="margin-bottom: 10px">
  1. Use proper indentations, white space and naming 
  conventions to make your code clean and readable
</div>
<div v-click style="margin-bottom: 10px">
  2. Avoid abbreviations and jargon
</div>
<div v-click style="margin-bottom: 10px">
  3. use comments to explain complex or non-obvious 
  parts of your code
</div>
<div v-click style="margin-bottom: 10px">
  4. Use descriptive variable names
</div>
<div v-click style="margin-bottom: 10px">
  5. Let your functions do one thing
</div>
<div v-click style="margin-bottom: 10px">
  6. Test your code
</div>



<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>


