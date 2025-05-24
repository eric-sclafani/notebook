
Template reference variables give you a way to declare a variable that references a value from an element in your template.

A template reference variable can refer to the following:
- A DOM element within a template
- An Angular component or directive
- A template ref from an [[Template Fragments#Referencing a template fragment|ng-template]]

You can declare a variable on an element in a template by adding an attribute that starts with the hash character (`#`) followed by the variable name.

```angular-html
<input #taskInput placeholder="Enter task name">
```

# Assigning values to template reference variables

Angular assigns a value to template variables based on the element on which the variable is declared.

If you **declare the variable on an Angular component**, the variable refers to the ==component instance.==

```angular-html
<my-datepickder #startDate />
```

If you declare the variable on an `<ng-template>` element, the variable refers to a **TemplateRef** instance which represents the template.

```angular-html
<ng-template #myFragment>
	<p>This is a template fragment</p>
</ng-template>
```

If you declare the variable on any other displayed element, the variable refers to the `HTMLElement` instance.

```angular-html
<input #taskInput placeholder="Enter task name">
```

# Querying template references

Template references can be used to "mark" an element for [[Queries|component and directive queries]]. 

```angular-ts
@Component({
	template: `<input #description value="Original description">`,		
})
export class AppComponent{
	input = viewChild<ElementRef|undefined>('description')
}
```

