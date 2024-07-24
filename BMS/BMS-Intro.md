---
id: BMS-Intro
aliases:
  - BMS-Intro
tags: []
---

# BMS-Intro

# Lithium Ion Batteries

> [!tip] To understand the purpose of BMS, we first need to know 
> - the basics of Lithium Ion batteries
> - their limitations
> - danger of using them inappropriately

> [!info] The main advantage of Lithium Ion cells/batteries over other options in market is that they can store more energy for given space and weight

## Disadvantages of Lithium Ion Batteries
- They can overheat and lose capacity *if discharged too fast*
- Their life cycle can deteriorate *if internal resistance goes beyond ==60 degrees Celsius==* or *if they are recharged when below 20 degrees Celsius*
- Can display performance issues *if discharged with temperature below 0 degrees Celsius*
- They can explode if overcharged above `4.2 V` or discharged below `2.7 V`
- Can also explode when recharging *if they have been used outside of operating range*
  > They are great batteries when used inside operating range
  > Life cycle can also be dramatically increased if they are used within limits specified in the datasheet

# Battery Packs

They are generally built from many battery modules which consist of several cells arranged in series or parallel to obtain desired voltage and capacity

# Role of BMS

Main role of BMS is to *protect the cells inside the [[#Battery Packs|packs]] from operating outside operating range* and to *optimize their cycle life by monitoring the cells(temperatures, voltages) while also controlling the balancing, charging and discharging process* according to ==predefined user settings==

- For several DIY projects which require less than `5kW` peak power, many BMS are available in market which can be good enough most of the time
- When some application requires more power than `5kW`, these BMS are not sufficient to protect the [[#Battery Packs|packs]]
  > this is because their parameters are fixed and not adjustable and they use solid state switches to disconnect battery from load in case of emergency 

