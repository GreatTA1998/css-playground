<div class="container">
	<button commandfor="my-dialog" command="show-modal" class="button">Open dialog</button>

	<dialog closedby="any" id="my-dialog" class="dialog">
		<div class="dialog-content">
			<h2 class="dialog-title">Modal Dialog</h2>
			<p class="dialog-description">
				Light dismiss behavior, but page is inert.
			</p>
		</div>
	</dialog>

	<div class="definitions">
		<div class="definition-item">
			<div class="definition-term">Modal</div>
			<div class="definition-desc">Interaction mode that blocks interaction with rest of UI until it's closed (in the same sense as Alan Kay's "No Modes" philosophy). Rest of page becomes <code>inert</code></div>
		</div>
		<div class="definition-item">
			<div class="definition-term">Popover</div>
			<div class="definition-desc">A declarative overlay primitive (often top-layer) intended for transient, dismissible UI. Manual vs auto.</div>
		</div>
		<div class="definition-item">
			<div class="definition-term">Dialog</div>
			<div class="definition-desc">Requires top visibility. Ever since popover was introduced, its unique advantage is that it can be modal (render the rest of the page inert)</div>
		</div>
		<div class="definition-note">
			The similarities between dialog and popover is that they want to be visible i.e. top visible layer, regardless of their true DOM position. The difference (despite overlap in behaviors) is that dialog is modal and popover isn't.
		</div>
	</div>

	<div class="sources">
		<div class="sources-group">
			<div class="sources-label">critical sources:</div>
			<div class="sources-links">
				<a href="https://hidde.blog/dialog-modal-popover-differences/" target="_blank" rel="noopener noreferrer" class="source-link">hidde.blog/dialog-modal-popover-differences</a>
				<a href="https://github.com/openui/open-ui/issues/741" target="_blank" rel="noopener noreferrer" class="source-link">github.com/openui/open-ui/issues/741</a>
			</div>
		</div>
		<div class="sources-group">
			<div class="sources-label">sources:</div>
			<div class="sources-links">
				<a href="https://web.dev/articles/inert" target="_blank" rel="noopener noreferrer" class="source-link">web.dev/articles/inert</a>
			</div>
		</div>
	</div>

	<div class="content">
		<h1 class="title">Dialog, Modal & Popover</h1>
		
		<section class="section">
			<h2 class="section-title">The Top Layer</h2>
			<p class="text">
				The top layer (as of July 2023, part of CSS's Positioned Layout Module, Level 4) is painted after the painting process described above, and the stuff within it is therefore on top of everything else. The top layer is not new in the web platform, but the ability for developers to promote elements to it is. Web pages have just one top layer. Within the top layer, elements are painted in the order they are added to the top layer (so shuffling them around involves adding/readding them).
			</p>
			<p class="text">
				Sometimes, developers add components just before the closing body tag to try and ensure that they are painted above other things (given nothing has a z-index > 0). The top layer introduces a new way to allow elements to be on top of everything else, regardless of where they are in the DOM or their z-index.
			</p>
			<p class="text">
				Another benefit of the top layer has to do with overflow. If your popup is in an element with overflow: hidden, that will cut it off. If it is promoted to the top layer, no cutting off will take place.
			</p>
			<p class="text">
				A downside of an element being in the top layer, is that it can't be positioned relative to things in the main document. I believe this is something modal dialogs usually don't need, but popovers would need a lot of the time. See also: Positioning anchored popovers.
			</p>
		</section>

		<section class="section">
			<h2 class="section-title">Modal vs Non-Modal</h2>
			<p class="text">
				Dialogs can be modal (dialog modal when shown with dialog.showModal()) or non modal (dialog when shown with dialog.show()). To avoid quirks, you will want to choose which of the two your dialog is, and only call one of these methods per dialog.
			</p>
			<p class="text">
				When dialogs with modal are modal, the browser will treat the content outside of the dialog as inert, and prevent keyboard focus from reaching web content outside of the dialog (if you use role="dialog", you have to do this yourself). If a dialog is not modal, the other content is not treated as inert.
			</p>
		</section>

		<section class="section">
			<h2 class="section-title">Popover</h2>
			<p class="text">
				The popover attribute is meant for UI components that are:
			</p>
			<ul class="list">
				<li>on top of other page content</li>
				<li>not always visible (eg just when they are relevant), also described as "short lived" or "ephemeral"</li>
				<li>usually displayed one at the time</li>
			</ul>
			<p class="text">
				As opposed to popovers, a popover doesn't come with a built-in role (this is partly why it is an attribute and not an element), you pick your own role. It can take on any role that makes sense, or none at all. Sometimes popovers could be (modeless) dialogs, in that case you could use dialog popover.
			</p>
			<p class="text">
				Examples include datepickers / calendar widgets, tooltips and toggletips, teaching UI (e.g. to point out parts of your interface when it is first shown), and action menus using role="menu".
			</p>
		</section>

		<section class="section">
			<h2 class="section-title">Characteristics</h2>
			<p class="text">
				Popovers are not modal. This is another major difference between popovers and dialogs. For this reason, it will be rare (but not impossible) for them to have a backdrop or focus trap.
			</p>
			<p class="text">
				Popovers can have 'light dismiss' behaviour, meaning they close by themselves, except when they are of the "manual" type. Manual popovers could be things like a "toast" notification that is dismissed via a timer or manual button.
			</p>
			<p class="text">
				Popovers, even if rare, can have a backdrop, which obscures content outside of it. This does not make the popover modal—as mentioned, popovers are non-modal. My recommendation is that if you're considering adding a backdrop to your popover, to also consider "oh wait, maybe this is a modal dialog instead". It might well be.
			</p>
			<p class="text">
				To position a popover, a very exciting proposal called CSS Anchor Positioning is in the works. As far as I understand it today, it would allow us popovers that automagically position in the most suitable place, avoiding collisions with the edge of the window. A bit like the Popper library does today, but built into the browser.
			</p>
		</section>
	</div>
</div>

<style>
	.container {
		max-width: 800px;
		margin: 0 auto;
		padding: 3rem 2rem;
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
		color: #0d0d0d;
	}

	.button {
		padding: 0.5rem 1rem;
		font-size: 0.875rem;
		font-weight: 500;
		color: #ffffff;
		background: #000000;
		border: none;
		border-radius: 6px;
		cursor: pointer;
		font-family: inherit;
	}

	.dialog {
		padding: 0;
		border: none;
		border-radius: 12px;
		box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
		max-width: 480px;
		width: calc(100% - 2rem);
		margin: auto;
		background: #ffffff;
	}

	.dialog::backdrop {
		background: rgba(0, 0, 0, 0.4);
		backdrop-filter: blur(4px);
	}

	.dialog-content {
		padding: 2rem;
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
	}

	.dialog-title {
		font-size: 1.25rem;
		font-weight: 600;
		color: #0d0d0d;
		margin: 0;
		line-height: 1.4;
		letter-spacing: -0.01em;
	}

	.dialog-description {
		font-size: 1rem;
		color: #6b7280;
		margin: 0;
		line-height: 1.6;
	}

	.definitions {
		margin-top: 3rem;
		padding: 1.5rem;
		background: #f9fafb;
		border-radius: 8px;
		border: 1px solid #e5e7eb;
		display: flex;
		flex-direction: column;
		gap: 1.25rem;
	}

	.definition-item {
		display: flex;
		flex-direction: column;
		gap: 0.375rem;
	}

	.definition-term {
		font-size: 0.875rem;
		font-weight: 600;
		color: #0d0d0d;
		letter-spacing: -0.01em;
	}

	.definition-desc {
		font-size: 0.875rem;
		color: #6b7280;
		line-height: 1.5;
	}

	.definition-desc code {
		font-size: 0.8125rem;
		font-family: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace;
		background: #ffffff;
		padding: 0.125rem 0.375rem;
		border-radius: 4px;
		border: 1px solid #e5e7eb;
		color: #0d0d0d;
	}

	.definition-note {
		margin-top: 0.5rem;
		padding-top: 1.25rem;
		border-top: 1px solid #e5e7eb;
		font-size: 0.875rem;
		color: #6b7280;
		line-height: 1.5;
	}

	.sources {
		margin-top: 1.5rem;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.sources-group {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	.sources-label {
		font-size: 0.8125rem;
		font-weight: 500;
		color: #6b7280;
		text-transform: lowercase;
	}

	.sources-links {
		display: flex;
		flex-direction: column;
		gap: 0.375rem;
	}

	.source-link {
		font-size: 0.875rem;
		color: #6b7280;
		text-decoration: none;
		line-height: 1.5;
	}

	.content {
		margin-top: 4rem;
	}

	.title {
		font-size: 2rem;
		font-weight: 600;
		color: #0d0d0d;
		margin: 0 0 3rem;
		line-height: 1.4;
		letter-spacing: -0.02em;
	}

	.section {
		margin-bottom: 3rem;
	}

	.section-title {
		font-size: 1.25rem;
		font-weight: 600;
		color: #0d0d0d;
		margin: 0 0 1rem;
		line-height: 1.4;
		letter-spacing: -0.01em;
	}

	.text {
		font-size: 1rem;
		color: #6b7280;
		line-height: 1.6;
		margin: 0 0 1rem;
	}

	.list {
		list-style: none;
		padding: 0;
		margin: 0 0 1rem;
	}

	.list li {
		font-size: 1rem;
		color: #6b7280;
		line-height: 1.6;
		padding-left: 1.5rem;
		position: relative;
		margin-bottom: 0.5rem;
	}

	.list li::before {
		content: '•';
		position: absolute;
		left: 0;
		color: #0d0d0d;
		font-weight: 500;
	}

	@media (max-width: 768px) {
		.container {
			padding: 2rem 1.5rem;
		}

		.title {
			font-size: 1.5rem;
			margin-bottom: 2rem;
		}

		.section {
			margin-bottom: 2rem;
		}

		.dialog-content {
			padding: 1.5rem;
		}

		.section-title {
			font-size: 1.125rem;
		}

		.text {
			font-size: 0.9375rem;
		}

		.list li {
			font-size: 0.9375rem;
		}

		.definitions {
			padding: 1.25rem;
			gap: 1rem;
		}

		.definition-term {
			font-size: 0.8125rem;
		}

		.definition-desc {
			font-size: 0.8125rem;
		}

		.definition-note {
			font-size: 0.8125rem;
		}

		.sources {
			margin-top: 1.25rem;
			gap: 0.875rem;
		}

		.sources-label {
			font-size: 0.75rem;
		}

		.source-link {
			font-size: 0.8125rem;
		}
	}
</style>
