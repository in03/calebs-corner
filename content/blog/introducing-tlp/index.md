+++
title = "TLP - Proposing a Novel and Deranged To-Do List Protocol"
description = "How I learned to stop worrying, and devise an embarrassingly slow TCP-inspired messaging protocol built on to-do lists."
date = 2025-01-15

[extra]
banner = "TLP-Blog-Banner.webp"
draft = true

[taxonomies]
tags = ["Project", "Journal"]
+++

## Elevator Pitch

In a world built on innovation, efficiency, and performance, **TLP**—**To-Do List Protocol** stands out...

For years, to-do list apps have been little more than demos for new front-end frameworks or hyperscaler spyware masquerading as productivity tools. TLP flips the script:  
1. **Compatible and Flexible**: Adapters are on the roadmap to support several popular to-do apps. MIT license and a rainy day means you can support any to-do app you like.
2. **Secure by Design**: By funnelling encrypted packets through your to-do list, you're holding a funhouse mirror up to hyperscaler data collection. If they’re going to harvest our data, let’s make them work for it.
3. **Feel Productive Again**: TLP doesn't break your tasks. Your app stays functional! Your personal tasks will be ignored and treated as corrupt packets. With hundreds of tasks being added and completed every second, you can check "dopamine" off your list. 


TLP is a groundbreaking, TCP-inspired messaging protocol that operates through to-do lists. Yes, you read that correctly. Your humble to-do list app has been granted super-powers: unnecessary partial re-implementation of the transport layer at the application layer! 

TLP is pure joy (and pure overhead).

**Checkout the project [here](https://github.com/in03/TLP/tree/main)!** 🚀

---

## What Is TLP?  
TLP is a satirical, semi-functional messaging protocol inspired by TCP that uses popular to-do list applications as the transport backbone. 

Packets are sent as to-do list tasks, ordered by due date and marked as complete when received. Everything is updated in real-time to simulate the mechanics of data packets traversing a network. 

Mix in some disruptive hypey buzz tech, some pseudo-encryption, and we have a winner! Think of it as the most inefficient way to communicate securely with your friends while simultaneously overwhelming your to-do app.


## How Does It Work?  
TLP is TCP inspired. It maps TCP packet fields to stock-standard to-do list task fields.

{% alert(tip=true) %}
Want to learn more about the anatomy of a TCP packet?
Check out this [GeeksForGeeks article](https://www.geeksforgeeks.org/tcp-ip-packet-format/).
{% end %}

| **TCP Packet Field**         | **To-Do Attribute**      | **Description**                                                                                                   |
|------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------|
| Source Port                  | Task Owner               | Represents the individual or entity initiating the task.                                                          |
| Destination Port             | Assigned User            | Indicates the person responsible for completing the task.                                                         |
| Sequence Number              | Task Due Date            | Encode sequence number in task due date to denote its order in a series                                           |
| Acknowledgment Number        | Task Dependencies        | Last received task as checked first checklist item, next expected task as unchecked following checklist item.     |
| ~~Data Offset~~              | ~~N/A~~                  | ~~Data offset (or header length) doesn't require re-implementation to retain expected behaviour.~~                |
| ~~Reserved~~                 | ~~N/A~~                  | ~~Reserved fields are placeholders without direct relevance to task attributes and can be excluded.~~             |
| Flag: URG                    | Task Priority (High)     | Set the task's priority level to 'High' to indicate urgency.                                                      |
| Flag: ACK                    | Task Completion Status   | Mark the task as complete to signify acknowledgment.                                                              |
| Flag: PSH                    | Task Priority (Important)| Set the task's priority level as 'High' to indicate PSH flag                                                      |
| Flag: RST                    | Task Reassignment        | Reassign the task to a different user to indicate a reset in responsibility.                                      |
| Flag: SYN                    | Task Coordination        | Add a checklist within the task for steps that require synchronization with others.                               |
| Flag: FIN                    | Task Closure             | Upon task completion, archive or remove the task to signify it's finished.                                        |
| Window Size                  | Todo List                | Group related tasks into a single list to represent the batch size.                                               |
| Checksum                     | Task Review              | Include a final checklist item for reviewing the task to ensure accuracy before completion.                       |
| Urgent Pointer               | Task Priority Indicator  | Use the task's priority setting to indicate urgency.                                                              |
| Options                      | Task Notes frontmatter   | Include any extra instructions or information in the task's notes as plaintext frontmatter.                       |
| ~~Padding~~                  | ~~N/A~~                  | ~~Padding is used for alignment in data structures and doesn't translate to task attributes; it can be omitted.~~ |
| Data (Payload)               | Task Notes               | Encrypted message as payload in task notes.                                                                       |

## Multicast??
While multicast is usually out of TCP territory, TLP leans heavily on the Application Layer and implements multicast.
If unicast, tasks are assigned to an individual. If multicast, the tasks are unassigned and all who are scoped receive the task.
Packet acknowledgement then becomes a race-condition. An exciting little competion in an otherwise very cooperative standard.

---

## What’s Next for TLP?  
In the coming posts, we'll explore more of TLP's tech spec and dream about the necessary components to get it off the ground, including, maybe:

- "Adapters" to interface TLP 🔌🔀 with popular to-do list apps. 
- Finding the Unicorn 🦄✨ of high-throughput to-do list apps.
- Overengineered blockchain integration 🧱⛓️ for authentication workflow.
- Different methods 


## Join the Revolution  
**To-Do List Protocol** is a call to action. It's a powerful reminder of what an industry shake-up can do. Do you know how diffusion based generative models work? The same way high school art assignments work. They throw something random at the canvas, interpret the results and learn from their mistakes. 

Join us! Don't chase the money. stop following the cues. Subvert the status quo.

Joiin us for the next installment of the **TLP Blog Series**!

[Explore TLP on GitHub](https://github.com/in03/TLP/tree/main)  

---

*Stay productive. Stay creative.*  
