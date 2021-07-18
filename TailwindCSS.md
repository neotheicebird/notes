https://daily.dev/blog/tailwind-css-from-zero-to-hero-up-and-running

## What?

Utility first CSS framework that lets you add styles to webpages without writing single line of custom CSS code. 

I don't see the tradeoff yet, maybe I would have to remember a lot of class names to get things right? Not sure.

##  Why?

There are frameworks like Bulma, Foundation, Bootstrap that are really popular, why one more? Here are the reasons:

1. You use only what you need. You can choose what styles you need to apply, code it and there it reflects immediately.
2. You _mostly_ dont need custom CSS
3. PurgeCSS will remove unused CSS
4. Built in responsive classes - I guess the move is from thinking in component terms to thinking in design element terms or in terms of classes
5. Tailwind CSS VSCode plugin will help auto-complete, guess this will help in 'should I remember a lot of stuff?' issue, which would be a bummer if not solved
6. support for dark mode, grids and they even have their own component library

## Principles

1. Using semantic CSS, we follow ideas like "seperation of concerns" and claim to make CSS that is seperate from HTML, such that we can change the CSS anytime without changing HTML. While it is true that HTML is independant of CSS, CSS isn't really independant of HTML. Its hard to achieve that. To explain further, take this example:

```scss
.box{
/* props */
	.card {
	  /* props */
	  button {
		...
	  }

  	}
}
```

We can see that CSS closely follow the structure of our HTML. So any change in HTML requires changes to CSS to work properly. To get rid of the structure we can use BEM, which is more loosely coupled.

```scss
/* Block component */
.btn {}

/* Element that depends upon the block */ 
.btn__price {}

/* Modifier that changes the style of the block */
.btn--orange {} 
.btn--big {}

```

2. Naming is hard. To make CSS that are semantically resonating, we need good names and names are hard to make. hmmm.. Ok!?
3. DRY is difficult, so we end up duplicating css. For example, if we have an "author-bio" component which is styled similar to a "article-preview" component, we end up duplicating code, or we can abstract patterns into a common class (making the style content agnostic) and `@extend` it into `.author-bio` and `.article-preview` or use a `@mixin`, still we have the cognitive load of naming the abstracted class
	a. Seperation of concerns by using names like `author-bio` and `article-preview` means CSS is dependant on HTML and CSS may not be DRY
	b. mixing concerns by using common classes like `media-card` and extending them means HTML is dependant on CSS?! meaning if CSS changes, HTML will suffer I guess. 
	
To tackle this, Tailwind was created by organically abstracting CSS abstractions that can be classified as either "content agnostic components" or "utility classes".

## Drawbacks

https://www.aleksandrhovhannisyan.com/blog/why-i-dont-like-tailwind-css/#but-first-why-does-tailwind-css-exist

1. Clumsy code. We end up with long horizontal code thats hard to read
2. Vendor lock-in. Once we start using Tailwind, it would be almost impossible to change frameworks. Tailwind seems like a fancy way to write inline styles. Its a bunch of abstractions (that could likely save time), that are coupled hard with the markup
3. Another readablility issue is that its hard to understand what the dev was thinking when they wrote something. class names gives us an idea on what we are dealing with in a particular line in code, but in a huge codebase if all we see are some jargons, fancy abbreviations, it becomes really hard to read the code. Tailwind can be great for prototyping, but bad for maintenance. Huge tech debt.
4. Tailwind doesn't play well with Dev Tools
5. Missing key features

## Conclusion

The idea of abstractions and using utility based CSS sounds great. But the way tailwind is used, its best practises doesn't sound good. I would still prefer to have semantics. Name element classes properly and add css attributes to those classes seperately, BUT I would like to `@apply` Tailwind/similar abstractions to save time. This is a feature that Tailwind gives but that goes against its own basic principles. If tailwind wasnt highly opiniated, this could be implemented better and would be a great feature.

So maybe, we look for such abstraction, utility first CSS framework and use a lot of `@mixin`, `@extend` and `@include` in our styling? That would solve many things that Tailwind proposes but doesn't solve?



