---
layout: default
title: Initial Design Report
---

## Jump to Section
- [ClimaPod](#climapod)
- [Design Concept](#design-concept)
- [Market Research](#market-research)
- [SWOT Analysis](#swot-analysis)
- [GANTT Chart](#GANTT-chart)

# ClimaPod
ClimaPod is a compact, low-cost humidity-controlled chamber designed to provide precise environmental control within small, enclosed volumes. In many laboratory and small-scale storage settings, humidity can fluctuate significantly due to ambient conditions, making it difficult to maintain stable moisture levels using passive methods alone. ClimaPod addresses this challenge by enabling users to specify target humidity set points and actively regulating the internal environment to maintain those conditions over time.

![ClimaPod](https://raw.githubusercontent.com/rithikar1510/CBE-3300B/main/ClimaPod.jpg)

# Design Concept
This diagram illustrates the overall design concept of ClimaPod and how humidity is actively regulated within the chamber. Two separate air streams are generated using individual air pumps. One air stream is passed through a container filled with desiccant beads, which removes moisture from the air and produces a dry air stream. The second air stream is bubbled through heated water, allowing the air to become saturated with moisture before entering the chamber as humidified air. These two streams are introduced into the chamber, where they mix to produce the desired humidity level.

A humidity sensor placed inside the chamber continuously monitors the internal environmental conditions and sends this information to the controller. Based on the difference between the measured humidity and the desired set point, the controller adjusts the relative flow rates of the dry and humid air streams. This closed-loop control strategy allows ClimaPod to maintain stable humidity over time, even in the presence of disturbances such as changes in ambient conditions or small leaks in the chamber.

![ClimaPod Design Concept](https://raw.githubusercontent.com/rithikar1510/CBE-3300B/main/Design_concept.jpg)

# Market Research

## Uses

Humidity chambers have a variety of different uses: plant growth, product testing, as a humidor, and for art or rare material preservation. Also known as climatic test chambers, products are placed inside the chamber and subjected to temperature and relative humidity cycles to determine their resistance. Different products include electronics, circuit boards, automotive components, pharmaceuticals, moisture-resistant materials, and consumer goods intended to have long-shelf lives. These chambers are important for product testing since they enable producers to simulate real-world situations. For example, circuit boards and electronics must be corrosion resistant to ensure longevity and efficiency, and moisture-resistant packaging and consumer goods must be tested for shelf life for FDA restrictions. 

## Market Trends

The global market for climatic test chambers is experiencing strong growth, with the 2024 valuation of the market being around 2.4 billion USD, with a CAGR of 2% CAGR to exceed 2.4 billion USD by 2032. Demand is growth due to strong product reliability and safety regulations and expectations from consumers and governments. Furthermore, growth of consumer electronics has pushed the demand for product validation, with especially strict standards for the aerospace and defense industries, who use these test chambers to test parts for their efficacy even in varying climatic conditions. 

Furthermore, integration of automation and artificial intelligence is pushing the abilities of these test chambers, where remote interfacing and real-time monitoring is pushing the abilities of these chambers. These chambers are thousands of dollars, with the smallest models starting at $5,000 for a 34 L system. However, these chambers also tend to couple temperature control, and they can go from -70 C to 183 C. Even the smallest models begin at $1000. 

## Where do we come in?

In the market, there is a gap for very small, budget climatic test chambers, especially chambers that only control humidity if temperature is not a variable of concern. The largest cost driver is the temperature control system, as the refridgeration system and heating elements are very expensive due to the number and the quality of the compressors and electronics required. Futhermore, the humidity sensors used for industry R&D have low ranges and margins of error in measurement, leading to a costlier subsystem. These two subsystems together are around 65% of the cost. The rest of the cost comes from the interface, control electronics, and the insulating material, which the most effective chambers used higher cost insulating materials. Our chamber hopes to occupy a space for a cheaper humidity chamber that only focuses on humidity, eliminating one subsystem. The total cost of our chamber would be $68.50, which is far below commercial options. This device would satisfy small scale product testing, such as for academic research or for home-use for preservation of plants, family heirlooms/delicate items, or DIY product development. 

# Price Breakdown

# SWOT Analysis

Strengths: Modular testing facility, allowing for flexibility in product testing and preservatiom, which allows for various, diverse uses. Could be for both industrial or average consumer usage.

Weaknesses: Lower accuracy and for price to remain low, temperature control system must be pared down or not as accurate as industrially available chambers. 

Opportunities: There is a gap in the market for lower cost humidity chambers. Furthermore, the market for climatic test chambers is on the rise, suggesting high demand for this product.

Threats: Industrially available chambers are highly sensitive and include the temperature system. The chamber industry is already well-established with a few producers, so new manufacturers will find it difficult to have a toehold in the market. 

# GANTT Chart
![Gantt Chart](https://raw.githubusercontent.com/rithikar1510/CBE-3300B/main/Gantt_Chart.jpg)

The Gantt chart outlines our planned project timeline, including key milestones from preliminary design through prototype development and final demonstration. Our initial prototype build is currently in progress, and we began circuit development last week. At this stage, we are on track with our planned schedule and have approximately two weeks remaining to complete the prototype. Moving forward, we will continue electrical prototyping and chamber assembly in parallel, followed by system integration, control tuning, and performance testing. The Gantt chart will be updated as the project progresses to reflect any adjustments to the timeline.

