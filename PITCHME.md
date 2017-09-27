---
```typescript
interface IUserFormControls extends IFormControls {
    name: FormControl;
    surname: FormControl;
}
@Component({
    selector: 'app-user-form',
    templateUrl: './user-form.component.html',
    styleUrls: ['./user-form.component.css'],
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserFormComponent extends FormComponent<IUserFormControls> IAfterFormCreate {
    @Input() public user: IUser;
    @Output() public onClose = new EventEmitter();

    setFormControls(): IUserFormControls {
        return {
            name: new FormControl(),
            surname: new FormControl()
        };
    }

    afterFormCreate(form: FormGroup): void {
        form.patchValue(this.user);
        this.registerForm('user-single-form');
    }
}
```
@[1]
@[11]
@[15-20]
@[9-14]
---