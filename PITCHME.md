---
```typescript
interface IUserFormControls extends IFormControls {
    name: FormControlTextfield;
    surname: FormControlTextfield;
    jobTitle: FormControlDropdown;
}
@Component({
    selector: 'user-form',
    templateUrl: './user-form.component.html',
    styleUrls: ['./user-form.component.css'],
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserFormComponent extends FormComponent<IUserFormControls> implements IAfterFormCreate {
    @Input() public user: IUser;
    @Output() public onClose = new EventEmitter();

    protected setFormControls(): IUserFormControls {
        return {
            name: new FormControlTextfield({...}),
            surname: new FormControlTextfield({...}),
            jobTitle: new FormControlDropdown({...})
        };
    }

    public afterFormCreate(form: FormGroup): void {
        form.patchValue(this.user);
        this.registerForm('user-data-form');
    }
}
```

@[1-5, 15-20] (Interface z info opisuący kontrolki w formularzu)
@[1, 15-20] (Interface i implementacja)
@[15-20] (Implementacja setFormControls)
@[11] 
@[9-14]
---