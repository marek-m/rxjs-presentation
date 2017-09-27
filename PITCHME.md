---
```typescript
interface IUserFormControls extends IFormControls {
    name: FormControlTextfield;
    surname: FormControlTextfield;
    jobTitle: FormControlDropdown;
}
@Component({ selector: 'user-form', ...})
export class UserFormComponent extends FormComponent<IUserFormControls> {
    // @Input() and @Output list goes here
    protected setFormControls(): IUserFormControls {
        return {
            name: new FormControlTextfield({...}),
            surname: new FormControlTextfield({...}),
            jobTitle: new FormControlDropdown({...})
        };
    }
}
```

@[1-5] (Interface z info opisuący kontrolki w formularzu)
@[7](Rozszerzamy klasę FormComponent (lub FormArrayComponent))
@[1-5, 9-14] (Implementacja setFormControls)
---