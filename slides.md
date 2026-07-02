---
# try also 'default' to start simple
# theme: eloc
title: Vulkan Triangle Speedrun (any%)
# apply UnoCSS classes to the current slide
class: text-center
transition: none
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 5min
fonts:
  sans: 'Hack Nerd Font Mono'
  serif: 'Hack Nerd Font Mono'
  mono: 'Hack Nerd Font Mono'
  local: 'Hack Nerd Font Mono'
  fallbacks: false
  provider: none
---

# high performance bash
### (tips you hopefully never need)

---
layout: quote
---

<style>
p {
font-size: 1.2rem !important;
}
</style>
> *"Premature optimization is the root of all evil."*
> -- some guy

---
layout: quote
---

![img](https://cdn.badcop.live/2026/07/02/58_52.png)

---
layout: quote
---

![img](https://cdn.badcop.live/2026/07/02/01_07.png)

---

<style>
.slidev-layout {
background: url('https://cdn.badcop.live/2026/07/02/12_39.png');
background-size: contain;
background-repeat: no-repeat;
}
h1, h2 {
color: black !important;
margin-top:1%;
}
h2 {
font-size: 2.2rem !important;
}
h1:nth-of-type(2) {
margin-top:11%;
}
h1:nth-of-type(3) {
margin-top:11.5%;
}
h2{
margin-top:11.5%;
}

</style>

<v-click>

# avoid forking

</v-click>

---

<style>
p, .shiki {
font-size: 1.4rem !important;
}
</style>

# what is forking?

bash *loves* processes

```bash
# do not run this!!

:(){ :|:& };:
```

---

<style>
p, .shiki {
font-size: 1.4rem !important;
}
</style>

# what is forking?

bash *loves* processes

```bash
# do not run this!!

fork() {
    fork | fork &
}
fork
```

---

<style>
p, li {
font-size: 1.5rem !important;
}
</style>

# what is forking?

the usual suspects

<v-clicks>

- `$()`
- `()`
- \`\`
- `&`
- `|`
- `|&`
- `coproc`
- any non-builtin command

</v-clicks>


---

<style>
.slidev-layout {
background: url('https://cdn.badcop.live/2026/07/02/12_39.png');
background-size: contain;
background-repeat: no-repeat;
}
h1, h2 {
color: black !important;
margin-top:1%;
}
h2 {
font-size: 2.2rem !important;
}
h1:nth-of-type(2) {
margin-top:11%;
}
h1:nth-of-type(3) {
margin-top:8.5%;
}
h2{
margin-top:11.5%;
}
img {
position: absolute;
right: 8%;
bottom: 36%;
transform: scale(1.5);
}

</style>

# avoid forking

<v-click>

# ... except sometimes

</v-click>

<v-click>

&nbsp;

![gru](https://cdn.badcop.live/2026/07/02/50_18.png)

</v-click>

---
layout: two-cols-header
---

<style>
.two-cols-header {
gap: 24px;
}
.shiki {
font-size: 1.2rem !important;
}
</style>

# math is slow

::left::

doesn't fork
```bash
#!/usr/bin/env bash
for((i=0; i <= 1000000; ++i)); do
  ((sum+=i))
done
echo "$sum"
```

::right::

<v-click>


forks a couple times
```bash
#!/usr/bin/env bash

seq 1000000 | paste -sd+ | bc



```

</v-click>

---

# math is slow

&nbsp;

![img](https://cdn.badcop.live/2026/07/02/31_59.png)

---

<style>
.slidev-layout {
background: url('https://cdn.badcop.live/2026/07/02/12_39.png');
background-size: contain;
background-repeat: no-repeat;
}
h1, h2 {
color: black !important;
margin-top:1%;
}
h2 {
font-size: 2.2rem !important;
}
h1:nth-of-type(2) {
margin-top:11%;
}
h1:nth-of-type(3) {
margin-top:9.5%;
}
h2{
margin-top:11.5%;
}

</style>

# avoid forking
# ... except sometimes

<v-click>

# fork _yourself_

</v-click>

---
layout: image-right
image: https://cdn.badcop.live/2026/07/02/01_18.png
---

<style>
p {
font-size: 1.5rem !important;
}
li {
font-size: 2.2rem !important;
}
</style>


# parallelization

you paid for the whole cpu

&nbsp;

<v-click>

split the input into smaller chunks

</v-click>


<v-clicks>

* `xargs -P`
* GNU `parallel`

</v-clicks>

---

<style>
p, .shiki {
font-size: 1.4rem !important;
}
</style>

# parallelization
brrrrrrrrrr

```bash
#!/usr/bin/env bash

step=100000
for((i=0; i<10; ++i)); do
  seq $((1+step*i)) $((step*(i+1))) \
    | paste -sd+ \
    | bc &
done \
  | paste -sd+ \
  | bc
```

---
layout: quote
---


# parallelization

&nbsp;

![img](https://cdn.badcop.live/2026/07/02/52_20.png)


---

<style>
.slidev-layout {
background: url('https://cdn.badcop.live/2026/07/02/12_39.png');
background-size: contain;
background-repeat: no-repeat;
}
h1, h2 {
color: black !important;
margin-top:1%;
}
h2 {
font-size: 2.2rem !important;
}
h1:nth-of-type(2) {
margin-top:11%;
}
h1:nth-of-type(3) {
margin-top:9.5%;
}
h2{
margin-top:11.5%;
}

</style>

# avoid forking
# ... except sometimes
# fork _yourself_

<v-click>

## port it all to C; call from bash

</v-click>

---
layout: quote
---

# rip bash

&nbsp;

![img](https://cdn.badcop.live/2026/07/02/57_47.png)
