---
title: Colours reference
description: Named colours, the 256 palette, truecolor, and how the active colour system downsamples.
---

<style>
.color-swatch {
  display: inline-block;
  min-width: 9.5rem;
  padding: 0.18rem 0.55rem;
  border-radius: 4px;
  font-family: var(--vp-font-family-mono);
  font-size: 0.78rem;
  line-height: 1.2;
  border: 1px solid rgba(127, 127, 127, 0.35);
  white-space: nowrap;
  text-align: left;
}
.color-table {
  width: 100%;
  display: table;
  border-collapse: collapse;
  font-size: 0.85rem;
}
.color-table th,
.color-table td {
  padding: 0.25rem 0.6rem;
  border-bottom: 1px solid var(--vp-c-divider);
  text-align: left;
  vertical-align: middle;
}
.color-table th { font-weight: 600; }
.color-table td:first-child { text-align: right; color: var(--vp-c-text-2); width: 3.5rem; }
.color-table .alias { color: var(--vp-c-text-2); }
</style>

# Colours reference

`TAnsiColor` is a value-type record describing a foreground or background
colour. Build them by name, by 256-palette index, by RGB, or by hex.

## Building colours

```pascal
// Named — the 16 ANSI colours plus the full xterm-256 palette
TAnsiColor.Red
TAnsiColor.Aqua
TAnsiColor.Lime
TAnsiColor.Cyan2
TAnsiColor.Grey

// 256-palette index (0-255)
TAnsiColor.FromIndex(202)

// 24-bit RGB
TAnsiColor.FromRGB(255, 136, 0)

// Hex
TAnsiColor.FromHex('#ff8800')
TAnsiColor.FromHex('ff8800')          // # is optional

// By name (case-insensitive; aliases like gray/grey, magenta/fuchsia,
// cyan/aqua all resolve)
TAnsiColor.FromName('orange1')
```

A zero-initialised `TAnsiColor` (no constructor call) means **default** —
"use the terminal's default for this slot". `IsDefault` returns `True`.

## In markup

The same names work as markup tokens (lowercase by convention):

```
[red]hi[/]
[bold cyan2]hi[/]
[#ff8800 on #1e90ff]warm on cool[/]
```

## The 16 ANSI base colours

Always available, regardless of terminal capability. Indices 0–15 map
directly to the SGR fg/bg codes:

<table class="color-table color-table-16">
<thead><tr><th>#</th><th>Swatch</th><th>Name</th><th>Hex</th><th>SGR fg</th><th>SGR bg</th></tr></thead>
<tbody>
<tr><td>0</td><td><span class="color-swatch" style="background:#000000;color:#fff">black</span></td><td><code>black</code></td><td><code>#000000</code></td><td>30</td><td>40</td></tr>
<tr><td>1</td><td><span class="color-swatch" style="background:#800000;color:#fff">maroon</span></td><td><code>maroon</code></td><td><code>#800000</code></td><td>31</td><td>41</td></tr>
<tr><td>2</td><td><span class="color-swatch" style="background:#008000;color:#fff">green</span></td><td><code>green</code></td><td><code>#008000</code></td><td>32</td><td>42</td></tr>
<tr><td>3</td><td><span class="color-swatch" style="background:#808000;color:#fff">olive</span></td><td><code>olive</code></td><td><code>#808000</code></td><td>33</td><td>43</td></tr>
<tr><td>4</td><td><span class="color-swatch" style="background:#000080;color:#fff">navy</span></td><td><code>navy</code></td><td><code>#000080</code></td><td>34</td><td>44</td></tr>
<tr><td>5</td><td><span class="color-swatch" style="background:#800080;color:#fff">purple</span></td><td><code>purple</code></td><td><code>#800080</code></td><td>35</td><td>45</td></tr>
<tr><td>6</td><td><span class="color-swatch" style="background:#008080;color:#fff">teal</span></td><td><code>teal</code></td><td><code>#008080</code></td><td>36</td><td>46</td></tr>
<tr><td>7</td><td><span class="color-swatch" style="background:#c0c0c0;color:#fff">silver</span></td><td><code>silver</code></td><td><code>#c0c0c0</code></td><td>37</td><td>47</td></tr>
<tr><td>8</td><td><span class="color-swatch" style="background:#808080;color:#fff">grey</span></td><td><code>grey <span class="alias">/ gray</span></code></td><td><code>#808080</code></td><td>90</td><td>100</td></tr>
<tr><td>9</td><td><span class="color-swatch" style="background:#ff0000;color:#fff">red</span></td><td><code>red</code></td><td><code>#ff0000</code></td><td>91</td><td>101</td></tr>
<tr><td>10</td><td><span class="color-swatch" style="background:#00ff00;color:#000">lime</span></td><td><code>lime</code></td><td><code>#00ff00</code></td><td>92</td><td>102</td></tr>
<tr><td>11</td><td><span class="color-swatch" style="background:#ffff00;color:#000">yellow</span></td><td><code>yellow</code></td><td><code>#ffff00</code></td><td>93</td><td>103</td></tr>
<tr><td>12</td><td><span class="color-swatch" style="background:#0000ff;color:#fff">blue</span></td><td><code>blue</code></td><td><code>#0000ff</code></td><td>94</td><td>104</td></tr>
<tr><td>13</td><td><span class="color-swatch" style="background:#ff00ff;color:#fff">fuchsia</span></td><td><code>fuchsia <span class="alias">/ magenta</span></code></td><td><code>#ff00ff</code></td><td>95</td><td>105</td></tr>
<tr><td>14</td><td><span class="color-swatch" style="background:#00ffff;color:#000">aqua</span></td><td><code>aqua <span class="alias">/ cyan</span></code></td><td><code>#00ffff</code></td><td>96</td><td>106</td></tr>
<tr><td>15</td><td><span class="color-swatch" style="background:#ffffff;color:#000">white</span></td><td><code>white</code></td><td><code>#ffffff</code></td><td>97</td><td>107</td></tr>
</tbody></table>

`Grey`, `Fuchsia` and `Aqua` each have a CSS-style alias (`Gray`,
`Magenta`, `Cyan`) that resolves to the same palette entry.

## The full xterm-256 palette

Indices 0–15 are the standard ANSI colours above, repeated here for
completeness. 16–231 form a 6×6×6 RGB cube using levels
`(0, 95, 135, 175, 215, 255)`. 232–255 are the 24-step grayscale ramp
`8 + (i − 232) × 10`. Names are taken verbatim from Spectre.Console's
generated palette so call sites match between the two libraries; the
trailing `_1`, `_2` suffixes mark repeated palette positions.

<table class="color-table">
<thead><tr><th>#</th><th>Swatch</th><th>Name</th><th>Hex</th></tr></thead>
<tbody>
<tr><td>0</td><td><span class="color-swatch" style="background:#000000;color:#fff">black</span></td><td><code>black</code></td><td><code>#000000</code></td></tr>
<tr><td>1</td><td><span class="color-swatch" style="background:#800000;color:#fff">maroon</span></td><td><code>maroon</code></td><td><code>#800000</code></td></tr>
<tr><td>2</td><td><span class="color-swatch" style="background:#008000;color:#fff">green</span></td><td><code>green</code></td><td><code>#008000</code></td></tr>
<tr><td>3</td><td><span class="color-swatch" style="background:#808000;color:#fff">olive</span></td><td><code>olive</code></td><td><code>#808000</code></td></tr>
<tr><td>4</td><td><span class="color-swatch" style="background:#000080;color:#fff">navy</span></td><td><code>navy</code></td><td><code>#000080</code></td></tr>
<tr><td>5</td><td><span class="color-swatch" style="background:#800080;color:#fff">purple</span></td><td><code>purple</code></td><td><code>#800080</code></td></tr>
<tr><td>6</td><td><span class="color-swatch" style="background:#008080;color:#fff">teal</span></td><td><code>teal</code></td><td><code>#008080</code></td></tr>
<tr><td>7</td><td><span class="color-swatch" style="background:#c0c0c0;color:#fff">silver</span></td><td><code>silver</code></td><td><code>#c0c0c0</code></td></tr>
<tr><td>8</td><td><span class="color-swatch" style="background:#808080;color:#fff">grey</span></td><td><code>grey <span class="alias">/ gray</span></code></td><td><code>#808080</code></td></tr>
<tr><td>9</td><td><span class="color-swatch" style="background:#ff0000;color:#fff">red</span></td><td><code>red</code></td><td><code>#ff0000</code></td></tr>
<tr><td>10</td><td><span class="color-swatch" style="background:#00ff00;color:#000">lime</span></td><td><code>lime</code></td><td><code>#00ff00</code></td></tr>
<tr><td>11</td><td><span class="color-swatch" style="background:#ffff00;color:#000">yellow</span></td><td><code>yellow</code></td><td><code>#ffff00</code></td></tr>
<tr><td>12</td><td><span class="color-swatch" style="background:#0000ff;color:#fff">blue</span></td><td><code>blue</code></td><td><code>#0000ff</code></td></tr>
<tr><td>13</td><td><span class="color-swatch" style="background:#ff00ff;color:#fff">fuchsia</span></td><td><code>fuchsia <span class="alias">/ magenta</span></code></td><td><code>#ff00ff</code></td></tr>
<tr><td>14</td><td><span class="color-swatch" style="background:#00ffff;color:#000">aqua</span></td><td><code>aqua <span class="alias">/ cyan</span></code></td><td><code>#00ffff</code></td></tr>
<tr><td>15</td><td><span class="color-swatch" style="background:#ffffff;color:#000">white</span></td><td><code>white</code></td><td><code>#ffffff</code></td></tr>
<tr><td>16</td><td><span class="color-swatch" style="background:#000000;color:#fff">grey0</span></td><td><code>grey0 <span class="alias">/ gray0</span></code></td><td><code>#000000</code></td></tr>
<tr><td>17</td><td><span class="color-swatch" style="background:#00005f;color:#fff">navyblue</span></td><td><code>navyblue</code></td><td><code>#00005f</code></td></tr>
<tr><td>18</td><td><span class="color-swatch" style="background:#000087;color:#fff">darkblue</span></td><td><code>darkblue</code></td><td><code>#000087</code></td></tr>
<tr><td>19</td><td><span class="color-swatch" style="background:#0000af;color:#fff">blue3</span></td><td><code>blue3</code></td><td><code>#0000af</code></td></tr>
<tr><td>20</td><td><span class="color-swatch" style="background:#0000d7;color:#fff">blue3_1</span></td><td><code>blue3_1</code></td><td><code>#0000d7</code></td></tr>
<tr><td>21</td><td><span class="color-swatch" style="background:#0000ff;color:#fff">blue1</span></td><td><code>blue1</code></td><td><code>#0000ff</code></td></tr>
<tr><td>22</td><td><span class="color-swatch" style="background:#005f00;color:#fff">darkgreen</span></td><td><code>darkgreen</code></td><td><code>#005f00</code></td></tr>
<tr><td>23</td><td><span class="color-swatch" style="background:#005f5f;color:#fff">deepskyblue4</span></td><td><code>deepskyblue4</code></td><td><code>#005f5f</code></td></tr>
<tr><td>24</td><td><span class="color-swatch" style="background:#005f87;color:#fff">deepskyblue4_1</span></td><td><code>deepskyblue4_1</code></td><td><code>#005f87</code></td></tr>
<tr><td>25</td><td><span class="color-swatch" style="background:#005faf;color:#fff">deepskyblue4_2</span></td><td><code>deepskyblue4_2</code></td><td><code>#005faf</code></td></tr>
<tr><td>26</td><td><span class="color-swatch" style="background:#005fd7;color:#fff">dodgerblue3</span></td><td><code>dodgerblue3</code></td><td><code>#005fd7</code></td></tr>
<tr><td>27</td><td><span class="color-swatch" style="background:#005fff;color:#fff">dodgerblue2</span></td><td><code>dodgerblue2</code></td><td><code>#005fff</code></td></tr>
<tr><td>28</td><td><span class="color-swatch" style="background:#008700;color:#fff">green4</span></td><td><code>green4</code></td><td><code>#008700</code></td></tr>
<tr><td>29</td><td><span class="color-swatch" style="background:#00875f;color:#fff">springgreen4</span></td><td><code>springgreen4</code></td><td><code>#00875f</code></td></tr>
<tr><td>30</td><td><span class="color-swatch" style="background:#008787;color:#fff">turquoise4</span></td><td><code>turquoise4</code></td><td><code>#008787</code></td></tr>
<tr><td>31</td><td><span class="color-swatch" style="background:#0087af;color:#fff">deepskyblue3</span></td><td><code>deepskyblue3</code></td><td><code>#0087af</code></td></tr>
<tr><td>32</td><td><span class="color-swatch" style="background:#0087d7;color:#fff">deepskyblue3_1</span></td><td><code>deepskyblue3_1</code></td><td><code>#0087d7</code></td></tr>
<tr><td>33</td><td><span class="color-swatch" style="background:#0087ff;color:#fff">dodgerblue1</span></td><td><code>dodgerblue1</code></td><td><code>#0087ff</code></td></tr>
<tr><td>34</td><td><span class="color-swatch" style="background:#00af00;color:#fff">green3</span></td><td><code>green3</code></td><td><code>#00af00</code></td></tr>
<tr><td>35</td><td><span class="color-swatch" style="background:#00af5f;color:#fff">springgreen3</span></td><td><code>springgreen3</code></td><td><code>#00af5f</code></td></tr>
<tr><td>36</td><td><span class="color-swatch" style="background:#00af87;color:#fff">darkcyan</span></td><td><code>darkcyan</code></td><td><code>#00af87</code></td></tr>
<tr><td>37</td><td><span class="color-swatch" style="background:#00afaf;color:#fff">lightseagreen</span></td><td><code>lightseagreen</code></td><td><code>#00afaf</code></td></tr>
<tr><td>38</td><td><span class="color-swatch" style="background:#00afd7;color:#fff">deepskyblue2</span></td><td><code>deepskyblue2</code></td><td><code>#00afd7</code></td></tr>
<tr><td>39</td><td><span class="color-swatch" style="background:#00afff;color:#fff">deepskyblue1</span></td><td><code>deepskyblue1</code></td><td><code>#00afff</code></td></tr>
<tr><td>40</td><td><span class="color-swatch" style="background:#00d700;color:#fff">green3_1</span></td><td><code>green3_1</code></td><td><code>#00d700</code></td></tr>
<tr><td>41</td><td><span class="color-swatch" style="background:#00d75f;color:#fff">springgreen3_1</span></td><td><code>springgreen3_1</code></td><td><code>#00d75f</code></td></tr>
<tr><td>42</td><td><span class="color-swatch" style="background:#00d787;color:#fff">springgreen2</span></td><td><code>springgreen2</code></td><td><code>#00d787</code></td></tr>
<tr><td>43</td><td><span class="color-swatch" style="background:#00d7af;color:#fff">cyan3</span></td><td><code>cyan3</code></td><td><code>#00d7af</code></td></tr>
<tr><td>44</td><td><span class="color-swatch" style="background:#00d7d7;color:#fff">darkturquoise</span></td><td><code>darkturquoise</code></td><td><code>#00d7d7</code></td></tr>
<tr><td>45</td><td><span class="color-swatch" style="background:#00d7ff;color:#000">turquoise2</span></td><td><code>turquoise2</code></td><td><code>#00d7ff</code></td></tr>
<tr><td>46</td><td><span class="color-swatch" style="background:#00ff00;color:#000">green1</span></td><td><code>green1</code></td><td><code>#00ff00</code></td></tr>
<tr><td>47</td><td><span class="color-swatch" style="background:#00ff5f;color:#000">springgreen2_1</span></td><td><code>springgreen2_1</code></td><td><code>#00ff5f</code></td></tr>
<tr><td>48</td><td><span class="color-swatch" style="background:#00ff87;color:#000">springgreen1</span></td><td><code>springgreen1</code></td><td><code>#00ff87</code></td></tr>
<tr><td>49</td><td><span class="color-swatch" style="background:#00ffaf;color:#000">mediumspringgreen</span></td><td><code>mediumspringgreen</code></td><td><code>#00ffaf</code></td></tr>
<tr><td>50</td><td><span class="color-swatch" style="background:#00ffd7;color:#000">cyan2</span></td><td><code>cyan2</code></td><td><code>#00ffd7</code></td></tr>
<tr><td>51</td><td><span class="color-swatch" style="background:#00ffff;color:#000">cyan1</span></td><td><code>cyan1</code></td><td><code>#00ffff</code></td></tr>
<tr><td>52</td><td><span class="color-swatch" style="background:#5f0000;color:#fff">darkred</span></td><td><code>darkred</code></td><td><code>#5f0000</code></td></tr>
<tr><td>53</td><td><span class="color-swatch" style="background:#5f005f;color:#fff">deeppink4</span></td><td><code>deeppink4</code></td><td><code>#5f005f</code></td></tr>
<tr><td>54</td><td><span class="color-swatch" style="background:#5f0087;color:#fff">purple4</span></td><td><code>purple4</code></td><td><code>#5f0087</code></td></tr>
<tr><td>55</td><td><span class="color-swatch" style="background:#5f00af;color:#fff">purple4_1</span></td><td><code>purple4_1</code></td><td><code>#5f00af</code></td></tr>
<tr><td>56</td><td><span class="color-swatch" style="background:#5f00d7;color:#fff">purple3</span></td><td><code>purple3</code></td><td><code>#5f00d7</code></td></tr>
<tr><td>57</td><td><span class="color-swatch" style="background:#5f00ff;color:#fff">blueviolet</span></td><td><code>blueviolet</code></td><td><code>#5f00ff</code></td></tr>
<tr><td>58</td><td><span class="color-swatch" style="background:#5f5f00;color:#fff">orange4</span></td><td><code>orange4</code></td><td><code>#5f5f00</code></td></tr>
<tr><td>59</td><td><span class="color-swatch" style="background:#5f5f5f;color:#fff">grey37</span></td><td><code>grey37 <span class="alias">/ gray37</span></code></td><td><code>#5f5f5f</code></td></tr>
<tr><td>60</td><td><span class="color-swatch" style="background:#5f5f87;color:#fff">mediumpurple4</span></td><td><code>mediumpurple4</code></td><td><code>#5f5f87</code></td></tr>
<tr><td>61</td><td><span class="color-swatch" style="background:#5f5faf;color:#fff">slateblue3</span></td><td><code>slateblue3</code></td><td><code>#5f5faf</code></td></tr>
<tr><td>62</td><td><span class="color-swatch" style="background:#5f5fd7;color:#fff">slateblue3_1</span></td><td><code>slateblue3_1</code></td><td><code>#5f5fd7</code></td></tr>
<tr><td>63</td><td><span class="color-swatch" style="background:#5f5fff;color:#fff">royalblue1</span></td><td><code>royalblue1</code></td><td><code>#5f5fff</code></td></tr>
<tr><td>64</td><td><span class="color-swatch" style="background:#5f8700;color:#fff">chartreuse4</span></td><td><code>chartreuse4</code></td><td><code>#5f8700</code></td></tr>
<tr><td>65</td><td><span class="color-swatch" style="background:#5f875f;color:#fff">darkseagreen4</span></td><td><code>darkseagreen4</code></td><td><code>#5f875f</code></td></tr>
<tr><td>66</td><td><span class="color-swatch" style="background:#5f8787;color:#fff">paleturquoise4</span></td><td><code>paleturquoise4</code></td><td><code>#5f8787</code></td></tr>
<tr><td>67</td><td><span class="color-swatch" style="background:#5f87af;color:#fff">steelblue</span></td><td><code>steelblue</code></td><td><code>#5f87af</code></td></tr>
<tr><td>68</td><td><span class="color-swatch" style="background:#5f87d7;color:#fff">steelblue3</span></td><td><code>steelblue3</code></td><td><code>#5f87d7</code></td></tr>
<tr><td>69</td><td><span class="color-swatch" style="background:#5f87ff;color:#fff">cornflowerblue</span></td><td><code>cornflowerblue</code></td><td><code>#5f87ff</code></td></tr>
<tr><td>70</td><td><span class="color-swatch" style="background:#5faf00;color:#fff">chartreuse3</span></td><td><code>chartreuse3</code></td><td><code>#5faf00</code></td></tr>
<tr><td>71</td><td><span class="color-swatch" style="background:#5faf5f;color:#fff">darkseagreen4_1</span></td><td><code>darkseagreen4_1</code></td><td><code>#5faf5f</code></td></tr>
<tr><td>72</td><td><span class="color-swatch" style="background:#5faf87;color:#fff">cadetblue</span></td><td><code>cadetblue</code></td><td><code>#5faf87</code></td></tr>
<tr><td>73</td><td><span class="color-swatch" style="background:#5fafaf;color:#fff">cadetblue_1</span></td><td><code>cadetblue_1</code></td><td><code>#5fafaf</code></td></tr>
<tr><td>74</td><td><span class="color-swatch" style="background:#5fafd7;color:#fff">skyblue3</span></td><td><code>skyblue3</code></td><td><code>#5fafd7</code></td></tr>
<tr><td>75</td><td><span class="color-swatch" style="background:#5fafff;color:#fff">steelblue1</span></td><td><code>steelblue1</code></td><td><code>#5fafff</code></td></tr>
<tr><td>76</td><td><span class="color-swatch" style="background:#5fd700;color:#fff">chartreuse3_1</span></td><td><code>chartreuse3_1</code></td><td><code>#5fd700</code></td></tr>
<tr><td>77</td><td><span class="color-swatch" style="background:#5fd75f;color:#fff">palegreen3</span></td><td><code>palegreen3</code></td><td><code>#5fd75f</code></td></tr>
<tr><td>78</td><td><span class="color-swatch" style="background:#5fd787;color:#fff">seagreen3</span></td><td><code>seagreen3</code></td><td><code>#5fd787</code></td></tr>
<tr><td>79</td><td><span class="color-swatch" style="background:#5fd7af;color:#fff">aquamarine3</span></td><td><code>aquamarine3</code></td><td><code>#5fd7af</code></td></tr>
<tr><td>80</td><td><span class="color-swatch" style="background:#5fd7d7;color:#000">mediumturquoise</span></td><td><code>mediumturquoise</code></td><td><code>#5fd7d7</code></td></tr>
<tr><td>81</td><td><span class="color-swatch" style="background:#5fd7ff;color:#000">steelblue1_1</span></td><td><code>steelblue1_1</code></td><td><code>#5fd7ff</code></td></tr>
<tr><td>82</td><td><span class="color-swatch" style="background:#5fff00;color:#000">chartreuse2</span></td><td><code>chartreuse2</code></td><td><code>#5fff00</code></td></tr>
<tr><td>83</td><td><span class="color-swatch" style="background:#5fff5f;color:#000">seagreen2</span></td><td><code>seagreen2</code></td><td><code>#5fff5f</code></td></tr>
<tr><td>84</td><td><span class="color-swatch" style="background:#5fff87;color:#000">seagreen1</span></td><td><code>seagreen1</code></td><td><code>#5fff87</code></td></tr>
<tr><td>85</td><td><span class="color-swatch" style="background:#5fffaf;color:#000">seagreen1_1</span></td><td><code>seagreen1_1</code></td><td><code>#5fffaf</code></td></tr>
<tr><td>86</td><td><span class="color-swatch" style="background:#5fffd7;color:#000">aquamarine1</span></td><td><code>aquamarine1</code></td><td><code>#5fffd7</code></td></tr>
<tr><td>87</td><td><span class="color-swatch" style="background:#5fffff;color:#000">darkslategray2</span></td><td><code>darkslategray2</code></td><td><code>#5fffff</code></td></tr>
<tr><td>88</td><td><span class="color-swatch" style="background:#870000;color:#fff">darkred_1</span></td><td><code>darkred_1</code></td><td><code>#870000</code></td></tr>
<tr><td>89</td><td><span class="color-swatch" style="background:#87005f;color:#fff">deeppink4_1</span></td><td><code>deeppink4_1</code></td><td><code>#87005f</code></td></tr>
<tr><td>90</td><td><span class="color-swatch" style="background:#870087;color:#fff">darkmagenta</span></td><td><code>darkmagenta</code></td><td><code>#870087</code></td></tr>
<tr><td>91</td><td><span class="color-swatch" style="background:#8700af;color:#fff">darkmagenta_1</span></td><td><code>darkmagenta_1</code></td><td><code>#8700af</code></td></tr>
<tr><td>92</td><td><span class="color-swatch" style="background:#8700d7;color:#fff">darkviolet</span></td><td><code>darkviolet</code></td><td><code>#8700d7</code></td></tr>
<tr><td>93</td><td><span class="color-swatch" style="background:#8700ff;color:#fff">purple_1</span></td><td><code>purple_1</code></td><td><code>#8700ff</code></td></tr>
<tr><td>94</td><td><span class="color-swatch" style="background:#875f00;color:#fff">orange4_1</span></td><td><code>orange4_1</code></td><td><code>#875f00</code></td></tr>
<tr><td>95</td><td><span class="color-swatch" style="background:#875f5f;color:#fff">lightpink4</span></td><td><code>lightpink4</code></td><td><code>#875f5f</code></td></tr>
<tr><td>96</td><td><span class="color-swatch" style="background:#875f87;color:#fff">plum4</span></td><td><code>plum4</code></td><td><code>#875f87</code></td></tr>
<tr><td>97</td><td><span class="color-swatch" style="background:#875faf;color:#fff">mediumpurple3</span></td><td><code>mediumpurple3</code></td><td><code>#875faf</code></td></tr>
<tr><td>98</td><td><span class="color-swatch" style="background:#875fd7;color:#fff">mediumpurple3_1</span></td><td><code>mediumpurple3_1</code></td><td><code>#875fd7</code></td></tr>
<tr><td>99</td><td><span class="color-swatch" style="background:#875fff;color:#fff">slateblue1</span></td><td><code>slateblue1</code></td><td><code>#875fff</code></td></tr>
<tr><td>100</td><td><span class="color-swatch" style="background:#878700;color:#fff">yellow4</span></td><td><code>yellow4</code></td><td><code>#878700</code></td></tr>
<tr><td>101</td><td><span class="color-swatch" style="background:#87875f;color:#fff">wheat4</span></td><td><code>wheat4</code></td><td><code>#87875f</code></td></tr>
<tr><td>102</td><td><span class="color-swatch" style="background:#878787;color:#fff">grey53</span></td><td><code>grey53 <span class="alias">/ gray53</span></code></td><td><code>#878787</code></td></tr>
<tr><td>103</td><td><span class="color-swatch" style="background:#8787af;color:#fff">lightslategrey</span></td><td><code>lightslategrey <span class="alias">/ lightslategray</span></code></td><td><code>#8787af</code></td></tr>
<tr><td>104</td><td><span class="color-swatch" style="background:#8787d7;color:#fff">mediumpurple</span></td><td><code>mediumpurple</code></td><td><code>#8787d7</code></td></tr>
<tr><td>105</td><td><span class="color-swatch" style="background:#8787ff;color:#fff">lightslateblue</span></td><td><code>lightslateblue</code></td><td><code>#8787ff</code></td></tr>
<tr><td>106</td><td><span class="color-swatch" style="background:#87af00;color:#fff">yellow4_1</span></td><td><code>yellow4_1</code></td><td><code>#87af00</code></td></tr>
<tr><td>107</td><td><span class="color-swatch" style="background:#87af5f;color:#fff">darkolivegreen3</span></td><td><code>darkolivegreen3</code></td><td><code>#87af5f</code></td></tr>
<tr><td>108</td><td><span class="color-swatch" style="background:#87af87;color:#fff">darkseagreen</span></td><td><code>darkseagreen</code></td><td><code>#87af87</code></td></tr>
<tr><td>109</td><td><span class="color-swatch" style="background:#87afaf;color:#fff">lightskyblue3</span></td><td><code>lightskyblue3</code></td><td><code>#87afaf</code></td></tr>
<tr><td>110</td><td><span class="color-swatch" style="background:#87afd7;color:#fff">lightskyblue3_1</span></td><td><code>lightskyblue3_1</code></td><td><code>#87afd7</code></td></tr>
<tr><td>111</td><td><span class="color-swatch" style="background:#87afff;color:#fff">skyblue2</span></td><td><code>skyblue2</code></td><td><code>#87afff</code></td></tr>
<tr><td>112</td><td><span class="color-swatch" style="background:#87d700;color:#fff">chartreuse2_1</span></td><td><code>chartreuse2_1</code></td><td><code>#87d700</code></td></tr>
<tr><td>113</td><td><span class="color-swatch" style="background:#87d75f;color:#fff">darkolivegreen3_1</span></td><td><code>darkolivegreen3_1</code></td><td><code>#87d75f</code></td></tr>
<tr><td>114</td><td><span class="color-swatch" style="background:#87d787;color:#000">palegreen3_1</span></td><td><code>palegreen3_1</code></td><td><code>#87d787</code></td></tr>
<tr><td>115</td><td><span class="color-swatch" style="background:#87d7af;color:#000">darkseagreen3</span></td><td><code>darkseagreen3</code></td><td><code>#87d7af</code></td></tr>
<tr><td>116</td><td><span class="color-swatch" style="background:#87d7d7;color:#000">darkslategray3</span></td><td><code>darkslategray3</code></td><td><code>#87d7d7</code></td></tr>
<tr><td>117</td><td><span class="color-swatch" style="background:#87d7ff;color:#000">skyblue1</span></td><td><code>skyblue1</code></td><td><code>#87d7ff</code></td></tr>
<tr><td>118</td><td><span class="color-swatch" style="background:#87ff00;color:#000">chartreuse1</span></td><td><code>chartreuse1</code></td><td><code>#87ff00</code></td></tr>
<tr><td>119</td><td><span class="color-swatch" style="background:#87ff5f;color:#000">lightgreen</span></td><td><code>lightgreen</code></td><td><code>#87ff5f</code></td></tr>
<tr><td>120</td><td><span class="color-swatch" style="background:#87ff87;color:#000">lightgreen_1</span></td><td><code>lightgreen_1</code></td><td><code>#87ff87</code></td></tr>
<tr><td>121</td><td><span class="color-swatch" style="background:#87ffaf;color:#000">palegreen1</span></td><td><code>palegreen1</code></td><td><code>#87ffaf</code></td></tr>
<tr><td>122</td><td><span class="color-swatch" style="background:#87ffd7;color:#000">aquamarine1_1</span></td><td><code>aquamarine1_1</code></td><td><code>#87ffd7</code></td></tr>
<tr><td>123</td><td><span class="color-swatch" style="background:#87ffff;color:#000">darkslategray1</span></td><td><code>darkslategray1</code></td><td><code>#87ffff</code></td></tr>
<tr><td>124</td><td><span class="color-swatch" style="background:#af0000;color:#fff">red3</span></td><td><code>red3</code></td><td><code>#af0000</code></td></tr>
<tr><td>125</td><td><span class="color-swatch" style="background:#af005f;color:#fff">deeppink4_2</span></td><td><code>deeppink4_2</code></td><td><code>#af005f</code></td></tr>
<tr><td>126</td><td><span class="color-swatch" style="background:#af0087;color:#fff">mediumvioletred</span></td><td><code>mediumvioletred</code></td><td><code>#af0087</code></td></tr>
<tr><td>127</td><td><span class="color-swatch" style="background:#af00af;color:#fff">magenta3</span></td><td><code>magenta3</code></td><td><code>#af00af</code></td></tr>
<tr><td>128</td><td><span class="color-swatch" style="background:#af00d7;color:#fff">darkviolet_1</span></td><td><code>darkviolet_1</code></td><td><code>#af00d7</code></td></tr>
<tr><td>129</td><td><span class="color-swatch" style="background:#af00ff;color:#fff">purple_2</span></td><td><code>purple_2</code></td><td><code>#af00ff</code></td></tr>
<tr><td>130</td><td><span class="color-swatch" style="background:#af5f00;color:#fff">darkorange3</span></td><td><code>darkorange3</code></td><td><code>#af5f00</code></td></tr>
<tr><td>131</td><td><span class="color-swatch" style="background:#af5f5f;color:#fff">indianred</span></td><td><code>indianred</code></td><td><code>#af5f5f</code></td></tr>
<tr><td>132</td><td><span class="color-swatch" style="background:#af5f87;color:#fff">hotpink3</span></td><td><code>hotpink3</code></td><td><code>#af5f87</code></td></tr>
<tr><td>133</td><td><span class="color-swatch" style="background:#af5faf;color:#fff">mediumorchid3</span></td><td><code>mediumorchid3</code></td><td><code>#af5faf</code></td></tr>
<tr><td>134</td><td><span class="color-swatch" style="background:#af5fd7;color:#fff">mediumorchid</span></td><td><code>mediumorchid</code></td><td><code>#af5fd7</code></td></tr>
<tr><td>135</td><td><span class="color-swatch" style="background:#af5fff;color:#fff">mediumpurple2</span></td><td><code>mediumpurple2</code></td><td><code>#af5fff</code></td></tr>
<tr><td>136</td><td><span class="color-swatch" style="background:#af8700;color:#fff">darkgoldenrod</span></td><td><code>darkgoldenrod</code></td><td><code>#af8700</code></td></tr>
<tr><td>137</td><td><span class="color-swatch" style="background:#af875f;color:#fff">lightsalmon3</span></td><td><code>lightsalmon3</code></td><td><code>#af875f</code></td></tr>
<tr><td>138</td><td><span class="color-swatch" style="background:#af8787;color:#fff">rosybrown</span></td><td><code>rosybrown</code></td><td><code>#af8787</code></td></tr>
<tr><td>139</td><td><span class="color-swatch" style="background:#af87af;color:#fff">grey63</span></td><td><code>grey63 <span class="alias">/ gray63</span></code></td><td><code>#af87af</code></td></tr>
<tr><td>140</td><td><span class="color-swatch" style="background:#af87d7;color:#fff">mediumpurple2_1</span></td><td><code>mediumpurple2_1</code></td><td><code>#af87d7</code></td></tr>
<tr><td>141</td><td><span class="color-swatch" style="background:#af87ff;color:#fff">mediumpurple1</span></td><td><code>mediumpurple1</code></td><td><code>#af87ff</code></td></tr>
<tr><td>142</td><td><span class="color-swatch" style="background:#afaf00;color:#fff">gold3</span></td><td><code>gold3</code></td><td><code>#afaf00</code></td></tr>
<tr><td>143</td><td><span class="color-swatch" style="background:#afaf5f;color:#fff">darkkhaki</span></td><td><code>darkkhaki</code></td><td><code>#afaf5f</code></td></tr>
<tr><td>144</td><td><span class="color-swatch" style="background:#afaf87;color:#fff">navajowhite3</span></td><td><code>navajowhite3</code></td><td><code>#afaf87</code></td></tr>
<tr><td>145</td><td><span class="color-swatch" style="background:#afafaf;color:#fff">grey69</span></td><td><code>grey69 <span class="alias">/ gray69</span></code></td><td><code>#afafaf</code></td></tr>
<tr><td>146</td><td><span class="color-swatch" style="background:#afafd7;color:#fff">lightsteelblue3</span></td><td><code>lightsteelblue3</code></td><td><code>#afafd7</code></td></tr>
<tr><td>147</td><td><span class="color-swatch" style="background:#afafff;color:#fff">lightsteelblue</span></td><td><code>lightsteelblue</code></td><td><code>#afafff</code></td></tr>
<tr><td>148</td><td><span class="color-swatch" style="background:#afd700;color:#000">yellow3</span></td><td><code>yellow3</code></td><td><code>#afd700</code></td></tr>
<tr><td>149</td><td><span class="color-swatch" style="background:#afd75f;color:#000">darkolivegreen3_2</span></td><td><code>darkolivegreen3_2</code></td><td><code>#afd75f</code></td></tr>
<tr><td>150</td><td><span class="color-swatch" style="background:#afd787;color:#000">darkseagreen3_1</span></td><td><code>darkseagreen3_1</code></td><td><code>#afd787</code></td></tr>
<tr><td>151</td><td><span class="color-swatch" style="background:#afd7af;color:#000">darkseagreen2</span></td><td><code>darkseagreen2</code></td><td><code>#afd7af</code></td></tr>
<tr><td>152</td><td><span class="color-swatch" style="background:#afd7d7;color:#000">lightcyan3</span></td><td><code>lightcyan3</code></td><td><code>#afd7d7</code></td></tr>
<tr><td>153</td><td><span class="color-swatch" style="background:#afd7ff;color:#000">lightskyblue1</span></td><td><code>lightskyblue1</code></td><td><code>#afd7ff</code></td></tr>
<tr><td>154</td><td><span class="color-swatch" style="background:#afff00;color:#000">greenyellow</span></td><td><code>greenyellow</code></td><td><code>#afff00</code></td></tr>
<tr><td>155</td><td><span class="color-swatch" style="background:#afff5f;color:#000">darkolivegreen2</span></td><td><code>darkolivegreen2</code></td><td><code>#afff5f</code></td></tr>
<tr><td>156</td><td><span class="color-swatch" style="background:#afff87;color:#000">palegreen1_1</span></td><td><code>palegreen1_1</code></td><td><code>#afff87</code></td></tr>
<tr><td>157</td><td><span class="color-swatch" style="background:#afffaf;color:#000">darkseagreen2_1</span></td><td><code>darkseagreen2_1</code></td><td><code>#afffaf</code></td></tr>
<tr><td>158</td><td><span class="color-swatch" style="background:#afffd7;color:#000">darkseagreen1</span></td><td><code>darkseagreen1</code></td><td><code>#afffd7</code></td></tr>
<tr><td>159</td><td><span class="color-swatch" style="background:#afffff;color:#000">paleturquoise1</span></td><td><code>paleturquoise1</code></td><td><code>#afffff</code></td></tr>
<tr><td>160</td><td><span class="color-swatch" style="background:#d70000;color:#fff">red3_1</span></td><td><code>red3_1</code></td><td><code>#d70000</code></td></tr>
<tr><td>161</td><td><span class="color-swatch" style="background:#d7005f;color:#fff">deeppink3</span></td><td><code>deeppink3</code></td><td><code>#d7005f</code></td></tr>
<tr><td>162</td><td><span class="color-swatch" style="background:#d70087;color:#fff">deeppink3_1</span></td><td><code>deeppink3_1</code></td><td><code>#d70087</code></td></tr>
<tr><td>163</td><td><span class="color-swatch" style="background:#d700af;color:#fff">magenta3_1</span></td><td><code>magenta3_1</code></td><td><code>#d700af</code></td></tr>
<tr><td>164</td><td><span class="color-swatch" style="background:#d700d7;color:#fff">magenta3_2</span></td><td><code>magenta3_2</code></td><td><code>#d700d7</code></td></tr>
<tr><td>165</td><td><span class="color-swatch" style="background:#d700ff;color:#fff">magenta2</span></td><td><code>magenta2</code></td><td><code>#d700ff</code></td></tr>
<tr><td>166</td><td><span class="color-swatch" style="background:#d75f00;color:#fff">darkorange3_1</span></td><td><code>darkorange3_1</code></td><td><code>#d75f00</code></td></tr>
<tr><td>167</td><td><span class="color-swatch" style="background:#d75f5f;color:#fff">indianred_1</span></td><td><code>indianred_1</code></td><td><code>#d75f5f</code></td></tr>
<tr><td>168</td><td><span class="color-swatch" style="background:#d75f87;color:#fff">hotpink3_1</span></td><td><code>hotpink3_1</code></td><td><code>#d75f87</code></td></tr>
<tr><td>169</td><td><span class="color-swatch" style="background:#d75faf;color:#fff">hotpink2</span></td><td><code>hotpink2</code></td><td><code>#d75faf</code></td></tr>
<tr><td>170</td><td><span class="color-swatch" style="background:#d75fd7;color:#fff">orchid</span></td><td><code>orchid</code></td><td><code>#d75fd7</code></td></tr>
<tr><td>171</td><td><span class="color-swatch" style="background:#d75fff;color:#fff">mediumorchid1</span></td><td><code>mediumorchid1</code></td><td><code>#d75fff</code></td></tr>
<tr><td>172</td><td><span class="color-swatch" style="background:#d78700;color:#fff">orange3</span></td><td><code>orange3</code></td><td><code>#d78700</code></td></tr>
<tr><td>173</td><td><span class="color-swatch" style="background:#d7875f;color:#fff">lightsalmon3_1</span></td><td><code>lightsalmon3_1</code></td><td><code>#d7875f</code></td></tr>
<tr><td>174</td><td><span class="color-swatch" style="background:#d78787;color:#fff">lightpink3</span></td><td><code>lightpink3</code></td><td><code>#d78787</code></td></tr>
<tr><td>175</td><td><span class="color-swatch" style="background:#d787af;color:#fff">pink3</span></td><td><code>pink3</code></td><td><code>#d787af</code></td></tr>
<tr><td>176</td><td><span class="color-swatch" style="background:#d787d7;color:#fff">plum3</span></td><td><code>plum3</code></td><td><code>#d787d7</code></td></tr>
<tr><td>177</td><td><span class="color-swatch" style="background:#d787ff;color:#fff">violet</span></td><td><code>violet</code></td><td><code>#d787ff</code></td></tr>
<tr><td>178</td><td><span class="color-swatch" style="background:#d7af00;color:#fff">gold3_1</span></td><td><code>gold3_1</code></td><td><code>#d7af00</code></td></tr>
<tr><td>179</td><td><span class="color-swatch" style="background:#d7af5f;color:#fff">lightgoldenrod3</span></td><td><code>lightgoldenrod3</code></td><td><code>#d7af5f</code></td></tr>
<tr><td>180</td><td><span class="color-swatch" style="background:#d7af87;color:#fff">tan</span></td><td><code>tan</code></td><td><code>#d7af87</code></td></tr>
<tr><td>181</td><td><span class="color-swatch" style="background:#d7afaf;color:#fff">mistyrose3</span></td><td><code>mistyrose3</code></td><td><code>#d7afaf</code></td></tr>
<tr><td>182</td><td><span class="color-swatch" style="background:#d7afd7;color:#fff">thistle3</span></td><td><code>thistle3</code></td><td><code>#d7afd7</code></td></tr>
<tr><td>183</td><td><span class="color-swatch" style="background:#d7afff;color:#fff">plum2</span></td><td><code>plum2</code></td><td><code>#d7afff</code></td></tr>
<tr><td>184</td><td><span class="color-swatch" style="background:#d7d700;color:#000">yellow3_1</span></td><td><code>yellow3_1</code></td><td><code>#d7d700</code></td></tr>
<tr><td>185</td><td><span class="color-swatch" style="background:#d7d75f;color:#000">khaki3</span></td><td><code>khaki3</code></td><td><code>#d7d75f</code></td></tr>
<tr><td>186</td><td><span class="color-swatch" style="background:#d7d787;color:#000">lightgoldenrod2</span></td><td><code>lightgoldenrod2</code></td><td><code>#d7d787</code></td></tr>
<tr><td>187</td><td><span class="color-swatch" style="background:#d7d7af;color:#000">lightyellow3</span></td><td><code>lightyellow3</code></td><td><code>#d7d7af</code></td></tr>
<tr><td>188</td><td><span class="color-swatch" style="background:#d7d7d7;color:#000">grey84</span></td><td><code>grey84 <span class="alias">/ gray84</span></code></td><td><code>#d7d7d7</code></td></tr>
<tr><td>189</td><td><span class="color-swatch" style="background:#d7d7ff;color:#000">lightsteelblue1</span></td><td><code>lightsteelblue1</code></td><td><code>#d7d7ff</code></td></tr>
<tr><td>190</td><td><span class="color-swatch" style="background:#d7ff00;color:#000">yellow2</span></td><td><code>yellow2</code></td><td><code>#d7ff00</code></td></tr>
<tr><td>191</td><td><span class="color-swatch" style="background:#d7ff5f;color:#000">darkolivegreen1</span></td><td><code>darkolivegreen1</code></td><td><code>#d7ff5f</code></td></tr>
<tr><td>192</td><td><span class="color-swatch" style="background:#d7ff87;color:#000">darkolivegreen1_1</span></td><td><code>darkolivegreen1_1</code></td><td><code>#d7ff87</code></td></tr>
<tr><td>193</td><td><span class="color-swatch" style="background:#d7ffaf;color:#000">darkseagreen1_1</span></td><td><code>darkseagreen1_1</code></td><td><code>#d7ffaf</code></td></tr>
<tr><td>194</td><td><span class="color-swatch" style="background:#d7ffd7;color:#000">honeydew2</span></td><td><code>honeydew2</code></td><td><code>#d7ffd7</code></td></tr>
<tr><td>195</td><td><span class="color-swatch" style="background:#d7ffff;color:#000">lightcyan1</span></td><td><code>lightcyan1</code></td><td><code>#d7ffff</code></td></tr>
<tr><td>196</td><td><span class="color-swatch" style="background:#ff0000;color:#fff">red1</span></td><td><code>red1</code></td><td><code>#ff0000</code></td></tr>
<tr><td>197</td><td><span class="color-swatch" style="background:#ff005f;color:#fff">deeppink2</span></td><td><code>deeppink2</code></td><td><code>#ff005f</code></td></tr>
<tr><td>198</td><td><span class="color-swatch" style="background:#ff0087;color:#fff">deeppink1</span></td><td><code>deeppink1</code></td><td><code>#ff0087</code></td></tr>
<tr><td>199</td><td><span class="color-swatch" style="background:#ff00af;color:#fff">deeppink1_1</span></td><td><code>deeppink1_1</code></td><td><code>#ff00af</code></td></tr>
<tr><td>200</td><td><span class="color-swatch" style="background:#ff00d7;color:#fff">magenta2_1</span></td><td><code>magenta2_1</code></td><td><code>#ff00d7</code></td></tr>
<tr><td>201</td><td><span class="color-swatch" style="background:#ff00ff;color:#fff">magenta1</span></td><td><code>magenta1</code></td><td><code>#ff00ff</code></td></tr>
<tr><td>202</td><td><span class="color-swatch" style="background:#ff5f00;color:#fff">orangered1</span></td><td><code>orangered1</code></td><td><code>#ff5f00</code></td></tr>
<tr><td>203</td><td><span class="color-swatch" style="background:#ff5f5f;color:#fff">indianred1</span></td><td><code>indianred1</code></td><td><code>#ff5f5f</code></td></tr>
<tr><td>204</td><td><span class="color-swatch" style="background:#ff5f87;color:#fff">indianred1_1</span></td><td><code>indianred1_1</code></td><td><code>#ff5f87</code></td></tr>
<tr><td>205</td><td><span class="color-swatch" style="background:#ff5faf;color:#fff">hotpink</span></td><td><code>hotpink</code></td><td><code>#ff5faf</code></td></tr>
<tr><td>206</td><td><span class="color-swatch" style="background:#ff5fd7;color:#fff">hotpink_1</span></td><td><code>hotpink_1</code></td><td><code>#ff5fd7</code></td></tr>
<tr><td>207</td><td><span class="color-swatch" style="background:#ff5fff;color:#fff">mediumorchid1_1</span></td><td><code>mediumorchid1_1</code></td><td><code>#ff5fff</code></td></tr>
<tr><td>208</td><td><span class="color-swatch" style="background:#ff8700;color:#fff">darkorange</span></td><td><code>darkorange</code></td><td><code>#ff8700</code></td></tr>
<tr><td>209</td><td><span class="color-swatch" style="background:#ff875f;color:#fff">salmon1</span></td><td><code>salmon1</code></td><td><code>#ff875f</code></td></tr>
<tr><td>210</td><td><span class="color-swatch" style="background:#ff8787;color:#fff">lightcoral</span></td><td><code>lightcoral</code></td><td><code>#ff8787</code></td></tr>
<tr><td>211</td><td><span class="color-swatch" style="background:#ff87af;color:#fff">palevioletred1</span></td><td><code>palevioletred1</code></td><td><code>#ff87af</code></td></tr>
<tr><td>212</td><td><span class="color-swatch" style="background:#ff87d7;color:#fff">orchid2</span></td><td><code>orchid2</code></td><td><code>#ff87d7</code></td></tr>
<tr><td>213</td><td><span class="color-swatch" style="background:#ff87ff;color:#fff">orchid1</span></td><td><code>orchid1</code></td><td><code>#ff87ff</code></td></tr>
<tr><td>214</td><td><span class="color-swatch" style="background:#ffaf00;color:#fff">orange1</span></td><td><code>orange1</code></td><td><code>#ffaf00</code></td></tr>
<tr><td>215</td><td><span class="color-swatch" style="background:#ffaf5f;color:#fff">sandybrown</span></td><td><code>sandybrown</code></td><td><code>#ffaf5f</code></td></tr>
<tr><td>216</td><td><span class="color-swatch" style="background:#ffaf87;color:#fff">lightsalmon1</span></td><td><code>lightsalmon1</code></td><td><code>#ffaf87</code></td></tr>
<tr><td>217</td><td><span class="color-swatch" style="background:#ffafaf;color:#000">lightpink1</span></td><td><code>lightpink1</code></td><td><code>#ffafaf</code></td></tr>
<tr><td>218</td><td><span class="color-swatch" style="background:#ffafd7;color:#000">pink1</span></td><td><code>pink1</code></td><td><code>#ffafd7</code></td></tr>
<tr><td>219</td><td><span class="color-swatch" style="background:#ffafff;color:#000">plum1</span></td><td><code>plum1</code></td><td><code>#ffafff</code></td></tr>
<tr><td>220</td><td><span class="color-swatch" style="background:#ffd700;color:#000">gold1</span></td><td><code>gold1</code></td><td><code>#ffd700</code></td></tr>
<tr><td>221</td><td><span class="color-swatch" style="background:#ffd75f;color:#000">lightgoldenrod2_1</span></td><td><code>lightgoldenrod2_1</code></td><td><code>#ffd75f</code></td></tr>
<tr><td>222</td><td><span class="color-swatch" style="background:#ffd787;color:#000">lightgoldenrod2_2</span></td><td><code>lightgoldenrod2_2</code></td><td><code>#ffd787</code></td></tr>
<tr><td>223</td><td><span class="color-swatch" style="background:#ffd7af;color:#000">navajowhite1</span></td><td><code>navajowhite1</code></td><td><code>#ffd7af</code></td></tr>
<tr><td>224</td><td><span class="color-swatch" style="background:#ffd7d7;color:#000">mistyrose1</span></td><td><code>mistyrose1</code></td><td><code>#ffd7d7</code></td></tr>
<tr><td>225</td><td><span class="color-swatch" style="background:#ffd7ff;color:#000">thistle1</span></td><td><code>thistle1</code></td><td><code>#ffd7ff</code></td></tr>
<tr><td>226</td><td><span class="color-swatch" style="background:#ffff00;color:#000">yellow1</span></td><td><code>yellow1</code></td><td><code>#ffff00</code></td></tr>
<tr><td>227</td><td><span class="color-swatch" style="background:#ffff5f;color:#000">lightgoldenrod1</span></td><td><code>lightgoldenrod1</code></td><td><code>#ffff5f</code></td></tr>
<tr><td>228</td><td><span class="color-swatch" style="background:#ffff87;color:#000">khaki1</span></td><td><code>khaki1</code></td><td><code>#ffff87</code></td></tr>
<tr><td>229</td><td><span class="color-swatch" style="background:#ffffaf;color:#000">wheat1</span></td><td><code>wheat1</code></td><td><code>#ffffaf</code></td></tr>
<tr><td>230</td><td><span class="color-swatch" style="background:#ffffd7;color:#000">cornsilk1</span></td><td><code>cornsilk1</code></td><td><code>#ffffd7</code></td></tr>
<tr><td>231</td><td><span class="color-swatch" style="background:#ffffff;color:#000">grey100</span></td><td><code>grey100 <span class="alias">/ gray100</span></code></td><td><code>#ffffff</code></td></tr>
<tr><td>232</td><td><span class="color-swatch" style="background:#080808;color:#fff">grey3</span></td><td><code>grey3 <span class="alias">/ gray3</span></code></td><td><code>#080808</code></td></tr>
<tr><td>233</td><td><span class="color-swatch" style="background:#121212;color:#fff">grey7</span></td><td><code>grey7 <span class="alias">/ gray7</span></code></td><td><code>#121212</code></td></tr>
<tr><td>234</td><td><span class="color-swatch" style="background:#1c1c1c;color:#fff">grey11</span></td><td><code>grey11 <span class="alias">/ gray11</span></code></td><td><code>#1c1c1c</code></td></tr>
<tr><td>235</td><td><span class="color-swatch" style="background:#262626;color:#fff">grey15</span></td><td><code>grey15 <span class="alias">/ gray15</span></code></td><td><code>#262626</code></td></tr>
<tr><td>236</td><td><span class="color-swatch" style="background:#303030;color:#fff">grey19</span></td><td><code>grey19 <span class="alias">/ gray19</span></code></td><td><code>#303030</code></td></tr>
<tr><td>237</td><td><span class="color-swatch" style="background:#3a3a3a;color:#fff">grey23</span></td><td><code>grey23 <span class="alias">/ gray23</span></code></td><td><code>#3a3a3a</code></td></tr>
<tr><td>238</td><td><span class="color-swatch" style="background:#444444;color:#fff">grey27</span></td><td><code>grey27 <span class="alias">/ gray27</span></code></td><td><code>#444444</code></td></tr>
<tr><td>239</td><td><span class="color-swatch" style="background:#4e4e4e;color:#fff">grey30</span></td><td><code>grey30 <span class="alias">/ gray30</span></code></td><td><code>#4e4e4e</code></td></tr>
<tr><td>240</td><td><span class="color-swatch" style="background:#585858;color:#fff">grey35</span></td><td><code>grey35 <span class="alias">/ gray35</span></code></td><td><code>#585858</code></td></tr>
<tr><td>241</td><td><span class="color-swatch" style="background:#626262;color:#fff">grey39</span></td><td><code>grey39 <span class="alias">/ gray39</span></code></td><td><code>#626262</code></td></tr>
<tr><td>242</td><td><span class="color-swatch" style="background:#6c6c6c;color:#fff">grey42</span></td><td><code>grey42 <span class="alias">/ gray42</span></code></td><td><code>#6c6c6c</code></td></tr>
<tr><td>243</td><td><span class="color-swatch" style="background:#767676;color:#fff">grey46</span></td><td><code>grey46 <span class="alias">/ gray46</span></code></td><td><code>#767676</code></td></tr>
<tr><td>244</td><td><span class="color-swatch" style="background:#808080;color:#fff">grey50</span></td><td><code>grey50 <span class="alias">/ gray50</span></code></td><td><code>#808080</code></td></tr>
<tr><td>245</td><td><span class="color-swatch" style="background:#8a8a8a;color:#fff">grey54</span></td><td><code>grey54 <span class="alias">/ gray54</span></code></td><td><code>#8a8a8a</code></td></tr>
<tr><td>246</td><td><span class="color-swatch" style="background:#949494;color:#fff">grey58</span></td><td><code>grey58 <span class="alias">/ gray58</span></code></td><td><code>#949494</code></td></tr>
<tr><td>247</td><td><span class="color-swatch" style="background:#9e9e9e;color:#fff">grey62</span></td><td><code>grey62 <span class="alias">/ gray62</span></code></td><td><code>#9e9e9e</code></td></tr>
<tr><td>248</td><td><span class="color-swatch" style="background:#a8a8a8;color:#fff">grey66</span></td><td><code>grey66 <span class="alias">/ gray66</span></code></td><td><code>#a8a8a8</code></td></tr>
<tr><td>249</td><td><span class="color-swatch" style="background:#b2b2b2;color:#fff">grey70</span></td><td><code>grey70 <span class="alias">/ gray70</span></code></td><td><code>#b2b2b2</code></td></tr>
<tr><td>250</td><td><span class="color-swatch" style="background:#bcbcbc;color:#fff">grey74</span></td><td><code>grey74 <span class="alias">/ gray74</span></code></td><td><code>#bcbcbc</code></td></tr>
<tr><td>251</td><td><span class="color-swatch" style="background:#c6c6c6;color:#000">grey78</span></td><td><code>grey78 <span class="alias">/ gray78</span></code></td><td><code>#c6c6c6</code></td></tr>
<tr><td>252</td><td><span class="color-swatch" style="background:#d0d0d0;color:#000">grey82</span></td><td><code>grey82 <span class="alias">/ gray82</span></code></td><td><code>#d0d0d0</code></td></tr>
<tr><td>253</td><td><span class="color-swatch" style="background:#dadada;color:#000">grey85</span></td><td><code>grey85 <span class="alias">/ gray85</span></code></td><td><code>#dadada</code></td></tr>
<tr><td>254</td><td><span class="color-swatch" style="background:#e4e4e4;color:#000">grey89</span></td><td><code>grey89 <span class="alias">/ gray89</span></code></td><td><code>#e4e4e4</code></td></tr>
<tr><td>255</td><td><span class="color-swatch" style="background:#eeeeee;color:#000">grey93</span></td><td><code>grey93 <span class="alias">/ gray93</span></code></td><td><code>#eeeeee</code></td></tr>
</tbody></table>

## Capability-aware downsampling

The active console's `Profile.Capabilities.ColorSystem` determines how a
`TAnsiColor` actually emits:

| `TColorSystem` | What's emitted |
| --- | --- |
| `NoColors` | Nothing. SGR escapes are stripped. |
| `Legacy` | 8 colours (SGR 30-37 / 40-47); brights downsampled. |
| `Standard` | 16 colours (adds SGR 90-97 / 100-107). |
| `EightBit` | 256 colours (`38;5;n` / `48;5;n`). |
| `TrueColor` | 24-bit RGB (`38;2;r;g;b` / `48;2;r;g;b`). |

A `TAnsiColor.FromRGB` value renders perfectly under `TrueColor`,
gracefully downsamples to the nearest 256 entry under `EightBit`, and
further to the closest of 16 / 8 colours under `Standard` / `Legacy`. You
get colour where it's supported and clean text where it isn't.

See [Capabilities](./capabilities.md) for how the colour system is detected.

## Helpers

```pascal
// Blend two colours by a 0..1 fade factor — used by the indeterminate
// progress bar pulse.
mid := TAnsiColor.Red.Blend(TAnsiColor.Yellow, 0.5);

// Compare two colours for equality
if c1.Equals(c2) then ...

// Render to debug-friendly string
s := c.ToString;     // e.g. 'rgb(255,136,0)' or '#ff8800'
```

## See also

- [Markup syntax](./markup-syntax.md) — using colours inside markup tags.
- [Styles](./styles.md) — combining colours with decorations into a `TAnsiStyle`.
- [Capabilities](./capabilities.md) — colour-system detection.
- [Canvas](../widgets/canvas.md) — for pixel-level RGB rendering.
