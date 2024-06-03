---
title: Software Documentation Best Practices With Examples
feed: show
---

[https://medium.com/@mithunmanoharan/software-documentation-best-practices-with-examples-8983b2bee99b](https://medium.com/@mithunmanoharan/software-documentation-best-practices-with-examples-8983b2bee99b)

![[assets/0LjjxYJ_z_4Raf_zx.jpg]]

**Software documentation** is a crucial part of working software.

Whether it’s API documentation, release notes, or customer-facing help content, you can’t afford to skip over the docs when it comes to shipping your new product.

“_Documentation is one of the most important parts of a software project. However, a lot of projects have little or no documentation to help their (potential) users use the software_,” says [Eric Holscher](https://www.ericholscher.com/blog/2013/jan/28/announcing-write-docs/), co-founder of Write the Docs.

[51% of people](https://econsultancy.com/83-of-online-shoppers-need-support-to-complete-a-purchase-stats/) prefer to receive technical support through a [knowledge base](https://helpjuice.com/blog/knowledge-base), and yet producing the relevant [technical documentation](https://helpjuice.com/blog/technical-documentation) can be a headache for many companies.

Luckily, there are many examples of software brands leading the way with documentation, and we’re going to take a look at some documentation best practices in this post.

Looking at other software documentation examples can inspire your own project, although your processes will be entirely your own.

## Types of Software Documentation

First and foremost, there are several types of software documentation, but the two main categories are product documentation and system documentation.

Each documentation type requires a slightly different approach since they are aimed at different audiences.

## User Documentation

- **How-to guides** — Problem-oriented, take the user through a series of steps to reach a real-world goal
- **Tutorials** — Learning-oriented, take the user through a series of steps to learn a concept
- **Reference docs** — Information-oriented, technical descriptions of the software (could include software design documents)
- **Explanations** — Understanding-oriented, they clarify or illuminate a particular topic for a user

![[assets/0LjKccDxUch4uAkNk.png]]

## For Developers Only

Your users can also be developers, and there are very specific types of documentation aimed at developers only. This is where it gets fascinating, but some developer-only docs include:

- **API documentation** — Reference documentation regarding making API calls and classes
- **Release notes** — Information describing the latest software or feature releases, and bug fixes
- **README** — A high-level overview of the software, usually alongside the source code
- **System documentation** — Documents describing the software system, including technical design documents, software requirements, and UML diagrams

![[assets/0W5sdjn0JyoU6k7w-.png]]

Consider code documentation that lives alongside the software, possibly in GitHub or similar tool — otherwise, none of your developers will read it.

Distinguish between internal software documentation and external end-user facing documentation — you typically need different writers with each.

We’re going to focus mostly on end-user facing documentation in this guide.

## Follow the Pre-Writing Exercise

You can also consider writing documentation as part of the “pre-writing” exercise for writing code — this is especially useful when you have multiple developers working on the same feature.

As [James Hood](http://jlhood.com/just-in-time-documentation/), Senior Software Developer at Amazon, says: “As the size and complexity of a feature grows, so does the amount of pre-writing required, especially when multiple developers are going to be collaborating on a feature.”

You’re not writing code per se, but you’re writing about the code you are going to use in order to solve a particular problem.

It’s like brainstorming the solutions you are going to use in your software before you actually implement any code.

## Identify Your Target Audience

If your target audience is not internal, then your audience is likely to be your customer base.

However, it may still be possible to segment your customer base down even further.

Splunk provides an in-depth guide in their book [The Product is the Docs](https://www.splunk.com/en_us/blog/splunklife/the-product-is-docs.html) on how to define your audience for technical writers.

Here’s a quote from Splunk: “_Reliable and accessible documentation requires thorough product knowledge. It also applies equally, if not more, on knowing your audience_.” — Splunk, p.275

This is an exercise that is useful not just for technical writers but also for other members of your company, including marketing, engineering, product, and support.

- Define your user. You can start with available user information and talk to customer-facing teams like support.
- Identify your user’s goals (e.g., install a database).
- Create audience personas.
- Create audience definitions (e.g., entry-level admin audience).
- Create use cases for the product (e.g., manage enterprise customers in a CRM system).
- Identify the correct delivery formats for your users (e.g., [FAQ](https://helpjuice.com/blog/write-faq-page), [wiki](https://helpjuice.com/blog/corporate-wiki), or knowledge base).
- Create content that is an appropriate scope and at the right level of detail.
- Identify appropriate users to provide feedback on your documentation.
- Conduct user research and communicate with users.

Remember, your software users may change over time. Repeat this exercise at least once a year.

## Follow Agile Documentation Practices

It’s increasingly common for software technical writers to work in Agile rather than Waterfall — The Agile approach to software documentation is used by [71% of companies](https://www.pmi.org/-/media/pmi/documents/public/pdf/learning/thought-leadership/pulse/pulse-of-the-profession-2017.pdf) for their software projects.

The definition of the [Agile Manifesto](https://agilemanifesto.org/) is:

- **Individuals and interactions** over processes and tools
- **Working software** over comprehensive documentation
- **Customer collaboration** over contract negotiation
- **Responding to change** over following a plan

The Docs Like Code methodology is a subset of Agile, and it means using the same tools and processes for the docs as you would for the software.

You’re aiming for the minimum possible amount of documentation. You can learn more about it in Anne Gentle’s book, “[Docs Like Code](https://www.docslikecode.com/)”. Anne says:

“GitHub and similar code systems avoid documentation ghettos because writers use the same tools that developers use. By adopting software techniques such as continuous integration, automated testing, and incremental improvement for docs, you get docs that are on par with the code itself.” — Anne Gentle, p. 16

Make your tech writers part of every scrum team instead of having a dedicated documentation team, since this encourages better collaboration between writers and developers.

When producing documentation, you will need to think about version control, source control, and collaboration.

Keep your documentation tasks in the same tools as the software, such as [Jira](https://www.atlassian.com/software/jira).

Instead of having your own Content Management System, use the same version control software that your developers use for the code.

## Just-In-Time Documentation

[Just In Time documentation](https://www.knowledgeowl.com/home/just-in-time-documentation) is a subset of Agile methodologies and originates from the Toyota Production System.

Instead of documenting every feature comprehensively, you produce articles as they become necessary based on your customer support tickets.

Naturally, this approach relates mostly to customer-facing product documentation rather than system docs or API docs aimed at technical folks. With develop-facing documentation, you want to try to be comprehensive.

[Minimum Viable Documentation](https://customersandcontent.com/2015/01/28/minimum-viable-documentation/) is an approach that makes sense when you have limited access to technical writing resources, and you have to produce relatively large amounts of documentation at speed.

It means that you aim for the minimum amount of content that you need to be successful, and no more.

[Agile for All](https://agileforall.com/new-to-agile-remember-one-thing-just-enough-just-in-time/) says: “We certainly don’t need “just in case” documentation, but I also believe it is a fallacy to think agile teams can be effective with zero documentation.

We need just enough documentation to make sure the team is successful.”

## Prioritize Documentation in the Development Process

Software documentation can easily fall under the radar until the last minute unless you make a concerted effort to prioritize through the Software Development Lifecycle.

Don’t allow developers to ship a feature unless it is accompanied by the appropriate documentation.

Hire technical writers who can promote the value of documentation within your company — Splunk has some great advice on how to do this.

Using the same tools as your development team can really help with this, since your documentation is far more visible.

For example, Splunk says: “_So what makes a technical writer exceptional? Resourcefulness and eagerness are key. When you screen tech writer candidates, look for a real appetite for discovery. The job, fundamentally, isn’t about writing or learning technology. It’s a relationship business, more like investigative journalism than anything else_.” — Splunk, p.634.

We’ll cover the importance of hiring technical writers in a later section.

[Docs Or It Didn’t Happen](https://www.ericholscher.com/blog/2013/jan/28/announcing-write-docs/) is part of the ethos of Write the Docs’ community — essentially meaning that no software or feature is complete without the relevant documentation.

This means empowering everyone on your development team to be a documentarian, from engineering, to product, to support, although not everyone will not directly write the documentation.

## Develop a Content Strategy

Documentation won’t appear by itself. Especially when you have a complex product that changes frequently, you need to take a conscious approach to how you produce documentation.

Your content strategy draws on your audience’s definitions in determining the approach you take.

Even if you use Just In Time methodologies, you have to think about your documentation as a whole.

For example, do a comprehensive audit of your docs — many companies have the same content living in more than one place, reams of outdated documentation, or content that is just plain wrong.

A documentation content strategy helps you keep on track, allocate resources, and manage your time. It will help you time your documentation alongside releases so you can have some idea of what’s coming up.

It can be particularly helpful for engineering teams, as this article by Increment explores.

According to [Increment](https://increment.com/documentation/primer-on-documentation-content-strategy/), “Content that maps to the audience’s needs lead to better comprehension and less confusion and frustration; it presents helpful information that explains confusing tasks and concepts, and anticipates where their challenges might occur.”

## Work Closely With Subject-Matter Experts

In software documentation, no one person has all the answers and your technical writers will be evaluated on their ability to get the most about Subject-Matter Experts.

As Splunk says: “Your relationship with your subject-matter experts is essential to your success. Many of these SMEs will be engineers.” — Splunk, p. 2218

Splunk recommends:

- Understanding the engineering workflow
- Learning the language of engineers so you can use it to speak to them
- Doing your homework on the software before meeting any engineer
- Familiarizing yourself with the terminology of the product

Your developers are likely to know the product so well that it may be hard to get practical answers from them that can translate into documentation.

Documentation should not only be written by technical writers — it should ideally be a synchronized collaboration between your documentation team, engineers, and support.

You need to make time for the technical review so that your SMEs can verify the accuracy of your documentation, as well as testing your docs on users.

## QA and Testing

We already touched on [Quality Assurance (QA)](https://searchsoftwarequality.techtarget.com/definition/quality-assurance) for your documentation a little when we talked about using Agile methodologies.

Documentation should not be published until it has been subject to technical verification, which is the point when QA tests your documentation to see if whatever solution you have presented works.

Your customers should not be the first testers of your documentation, or you have failed to provide working docs.

Build this testing phase into your Agile process so that team members leave time to test before the product or feature ships.

You also need to include a phase where the relevant members of the product or engineering team review your documentation to check for technical accuracy, so this is where strong relationships between teams come in very handy.

Make sure you formalize your process to ensure all team members appreciate the importance of the documentation.

Do not allow the code to merge until documentation has been approved.

## Customer Feedback Loop

Closing the customer feedback loop is all part of the Agile process. Agile development should be continuous communication between the development team and the customer, at all stages of the process.

From beta testing with a research group, to feedback on the first release, and ongoing feedback from customers, you need to keep your finger on the pulse. It doesn’t always have to be a formal research group.

There are many ways to collect feedback:

- Knowledge base contact form
- Enabling comments on documentation
- Allow users to “rate” the usefulness of the content
- Check your knowledge base data analytics
- Customer support tickets
- Social media

Closing the feedback loop means connecting the feedback with the right internal department.

If customer support gets an issue with the software, this needs to be passed on to the engineering team and logged as a bug or a feature request.

If there’s a problem with how the product has been marketed, this needs to be passed on to the marketing team.

You get the idea!

## Create a Style Guide

You absolutely need a style guide for your software documentation, just like you would for your marketing activities. The main things you should cover in your style guide are:

- Standardized terminology (how to refer to your company and software)
- Voice & tone
- Page formatting (use of headers, paragraphs, lists)
- Guide on word choice (should it be internet or Internet — obviously the former!)
- Use of visuals and video

You can refer to some well-known software style guides like the [Rackspace Style Guide](https://developer.rackspace.com/docs/style-guide/), or the [Microsoft Style Guide](https://docs.microsoft.com/en-us/style-guide/welcome/).

For advice on grammatical choices, such as whether to use the Oxford Comma, you can check standard style guides such as the [Chicago Manual of Style](https://www.chicagomanualofstyle.org/home.html).

Adopt a coherent writing style, especially if you are using teams of writers who all write in different styles. This all adds to the User Experience of the documentation, which we will discuss later.

Software documentation templates can come in really handy so your writers don’t have to consult the style guide every time.

And you can either make them yourself, search online, or find them in enterprise content tools like Atlassian Confluence.

![[assets/0QPf7TldYuGUSHUcX.png]]

([Source](https://styleguide.mailchimp.com/writing-technical-content/))

## Think About User Experience

You will need to consider the User Experience (UX) of your documentation, especially customer-facing help content.

Just writing the content is half the battle — how do customers feel when they encounter your knowledge base? Are they able to easily find what they need?

If you invest in the proper knowledge base software like Helpjuice, you will have built-in templates with UX designed just for documentation users.

## Information Architecture

The Information Architecture of your knowledge base (IA) follows the same principles as for any other IA projects, and is an aspect of User Experience.

This is where professional technical writing help is very important, which we’ll cover in a later section.

1. The structural design of shared information environments.
2. The combination of organization, labeling, search, and navigation systems within websites and intranets.
3. The art and science of shaping information products and experiences to support usability and inability.
4. An emerging discipline and community of practice focused on bringing principles of design and architecture to the digital landscape

Check out this API documentation portal from Slack:

![[assets/0HtvYX8k5cyhDd-Ww.gif]]

## Inclusivity and Accessibility

When you get to a certain point in your documentation, you need to seriously consider how people with different needs will be able to use your documentation.

For example, consider whether your users are from international audiences when actually writing content. You want to avoid the use of idioms and references that they might not understand.

Accessibility relates to the User Experience of the documentation tool itself. For example, consider whether it will be accessible to a person using a screen reader, which will read the test aloud to a person using it.

Images with text overlaid are not accessible, so think about your screenshots and make sure they have accompanying text.

Take into consideration the contrasting colors of your knowledge base design, and how you style links, to ensure other users with visual impairments can engage with your site successfully. Take this example from Write the Docs’ website:

![[assets/04JsAxZb-ImHFBqvV.png]]

The site design is very clear, easy to use, with underlined links and short paragraphs. The black and white color scheme provides a high contrast for visually impaired users.

## Content Discoverability

Consider how customers arrive at your knowledge base in the first place. Very few customers will consider your knowledge base as a whole, and hardly anyone will arrive at your carefully constructed homepage.

Consider Every Page is Page One principles as described by Mark Baker. According to EPPO, people “forage” for information like animals searching for food, rather than learning in a linear fashion as you would with a book.

There is no “beginning: to start from.

Mark says: “There is no “Start Here” page for the Web. Wherever you dip your toe into the Web, that is your page one. We can’t avoid this. Whether you are a reader or a writer, and whether you like it or not, that is the way the Web works. Every page is page one.” — Mark Baker, p.141

Your software documentation is no good if nobody can find it, but there are a number of ways to promote your content. In fact, Google’s search engine is often “page one” for many users.

Your knowledge base software should be indexable by search engines, with all the correct meta tags. You should also link to your documentation from your software app, since this is where users will naturally get stuck.

If possible, make use of contextual help which is served up whenever customers need it.

For example, if customers are having trouble with their billing, ensure a link takes them to a page with billing documentation that can help solve their problem.

Here’s an example of contextual help inside Slack:

![[assets/0XhvWkPJgtM9REI5S.png]]

## Choose the Appropriate Software

To deliver the documentation to your users, you need the appropriate [software documentation tool](https://helpjuice.com/blog/software-documentation-tools). You might be fine using GitHub and a Static Site Generator, or you might need a tool with better User Experience.

[Knowledge base software](https://helpjuice.com/knowledge-base-software) like [Helpjuice](https://www.helpjuice.com/?utm_source=blog&utm_medium=incontent&utm_campaign=software_documentation) enables you to easily create and publish documentation in a stylish website optimized for search and discovery.

Check out this example of a Helpjuice knowledge base by WealthBar:

![[assets/0UX1SNkm7B5vSaV2M.png]]

## Version Control

As you iterate on versions of your product or features, you’ll need to keep track of the different versions of your documentation.

This can even mean creating entirely new knowledge bases and maintaining several KBs for different versions of the product.

Helpjuice allows you to create different versions of your documentation and even switch between them in the editor.

Many companies need to keep different versions of their documentation live at the same time for customers who are using different releases.

Keeping track of your documentation tasks in collaboration tools like Asana or Trello is also essential. Whatever your tool, make sure everyone is using it for maximum productivity.

## Lightweight Markup Languages

Consider whether you want to work in a lightweight markup language. This can be especially useful if you’re working with Docs like Code and you want to keep your content alongside the codebase.

Tools like GitHub allow you to work in Markdown and generally it’s necessary for your technical writers to be able to use markup.

There are a few different options when it comes to markup languages:

- Markdown
- reStructuredText
- AsciiDoc

One of the advantages of working in a lightweight markup language is that content can be easily styled and presented in a visually appealing way.

Also, if you need to migrate your content over to a different system, if it’s written in markup then you can keep all the formatting in your content.

Here’s a preview of Markdown in text editor Atom:

![[assets/0zC7LMvMrJIllsrxg.png]]

([Source](https://atom.io/packages/markdown-preview))

## Hire Professional Technical Writers

There’s nothing wrong with your developers writing documentation if necessary, but at some point, it’s not the best use of their time.

This is where professional technical writers come in. You can outsource your technical writing to an agency or you can hire in-house expertise.

One thing you can’t ignore is the importance of professional technical writing skills.

If you want to achieve anything like success with your User Experience, Information Architecture, and understanding of your audience, then professional tech writing experience is essential.

Your documentation gives an important impression to your customers that you care about their success with your software and you have provided help for when they run into problems.

Unprofessional documentation can also be off-putting to potential customers.

## Continuous Improvement

Documentation is never done, and you’ll always have to iterate on your efforts. Set time aside to review the documentation, identify missing documentation, or improve documentation that is frequently used.

This relates to the customer feedback loop. Quickly act on comments from your customers that tell you your documentation is failing to solve a problem.

Make the time to talk to your support agents about what documentation they might find useful, and even empower them to create it themselves!

## Final Remarks

Good documentation practices are just as important a part of the software as the code. Build the documentation into your development process and try to use the same Agile methodologies.
