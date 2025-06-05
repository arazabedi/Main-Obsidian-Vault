- Revolves around never trusting user input, especially when rendering it into the DOM.
- The most effective step is contextual output encoding.
- Escape all characters such as `&` so that the browser doesn't interpret them as part of teh page's structure.
- Frameworks like React automatically escape all content by default unless you user `dangerouslySetInnerHTML` which should be avoided unless absolutely necessary,.

- Content Security Policy is another important layer.
- Use a strict CSP to disallow inline scripts and restrict sources to limit the impact of any XSS that does make it through.

- Alway validate and sanitise user input, especially of you're allowing HTML (e.g. in rich-text fields). 
- Libraries such as DOMPurify can clean user-submitted HTML.

- Avoid dangerous practices such as `innerHTML`, `document.write`, or `eval`
- Keep third-party libraries updated to avoid introducing vulnerabilities.

- In short:
		Encode output by default, use a strong CSP, sanitise rich input, and adopt secure coding habits to keep the browser DOM un-compromised.

