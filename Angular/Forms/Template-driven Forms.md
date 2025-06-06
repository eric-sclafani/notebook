Template-driven forms use [[Two-way Binding|two-way data binding]] to update the data model in the component as changes are made.

It's often said that template-driven forms are not great for larger forms. I'm not sure how true this is, as [Ward Bell's Youtube video](https://www.youtube.com/watch?v=L7rGogdfe2Q) goes into great detail on how to better use these forms instead of reactive. It is, however, all up to personal preference.

Like [[Reactive Forms|reactive forms]], template-driven forms use [[Form Control|form controls]]. But in this case, instead of you defining the form yourself, Angular constructs the form for you.

# Build a template-driven form

Template-driven forms rely on directives defined in the `FormsModule`.

| Directives     | Details                                                                                                                                                                                                                                                                         |
| :------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `NgModel`      | Reconciles value changes in the attached form element with changes in the data model, allowing you to respond to user input with input validation and error handling.                                                                                                           |
| `NgForm`       | Creates a top-level `FormGroup` instance and binds it to a `<form>` element to track aggregated form value and validation status. As soon as you import `FormsModule`, this directive becomes active by default on all `<form>` tags. You don't need to add a special selector. |
| `NgModelGroup` | Creates and binds a `FormGroup` instance to a DOM element.                                                                                                                                                                                                                      |
This note will not go into detail about template-driven forms (at least, not yet). For now, see the [official Angular documentation for further details](https://angular.dev/guide/forms/template-driven-forms).