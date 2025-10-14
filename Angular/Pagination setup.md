
**First we add a paginator:

```
@Component({

selector: 'app-paginator',

imports: [],

templateUrl: './paginator.html',

styleUrl: './paginator.css',

})

export class Paginator {

totalPages = input<number>(0);

pageNumber = model<number>(1);

pageSize = model<number>(5);

totalCount = input<number>(0);

pageSizeOptions = input<number[]>([5, 10, 20, 50]);

  

pageChange = output<{ pageNumber: number; pageSize: number }>();

lastItemIndex = computed(() => {

return Math.min(this.pageNumber() * this.pageSize(), this.totalCount());

});

  

onPageChange(newPage?: number, pageSize?: EventTarget | null) {

if (newPage) this.pageNumber.set(newPage);

if (pageSize) {

const size = Number((pageSize as HTMLSelectElement).value);

this.pageSize.set(size);

}

this.pageChange.emit({ pageNumber: this.pageNumber(), pageSize: this.pageSize() });

}

}



<div class="flex items-center w-full gap-3 py-3">

<span>Items per page:</span>

<select class="select w-24" (change)="onPageChange(undefined, $event.target)">

@for (size of pageSizeOptions(); track $index) {

<option [selected]="pageSize() === size" [value]="size">{{ size }}</option>

}

</select>

  

<div class="text-sm">

{{ (pageNumber() - 1) * pageSize() + 1 }} - {{ lastItemIndex() }} of {{ totalCount() }}

</div>

  

<div class="flex items-center gap-2">

<button

class="btn btn-circle btn-ghost"

[disabled]="pageNumber() === 1"

(click)="onPageChange(pageNumber() - 1)"

>

&larr;

</button>

<button

class="btn btn-circle btn-ghost"

[disabled]="pageNumber() >= totalPages()"

(click)="onPageChange(pageNumber() + 1)"

>

&rarr;

</button>

</div>

</div>
```


**Then we add our models:

```
export interface Pagination {

currentPage: number;

pageSize: number;

totalCount: number;

totalPages: number;

}

  

export interface PaginatedResult<T> {

items: T[];

metadata: Pagination;

}
```

**And we hook it up in api calling service:

```
getMembers(pageNumber: number = 1, pageSize: number = 5) {

let params = new HttpParams();

params = params.append('pageNumber', pageNumber.toString());

params = params.append('pageSize', pageSize.toString());

//params is the name of the property that HttpClient expects, that's why we pass it like this. if we named our variable differently it would not work

return this.httpClient.get<PaginatedResult<Member>>(`${this.apiUrl}/members`, { params });

}
```

**Finally we set it up where the list of items is:

```
@Component({

selector: 'app-member-list',

imports: [MemberCard, Paginator],

templateUrl: './member-list.html',

styleUrl: './member-list.css',

})

export class MemberList implements OnInit {

private readonly memberService = inject(MemberService);

  

protected paginatedMembers = signal<PaginatedResult<Member> | null>(null);

pageNumber = 1;

pageSize = 5;

  

loadMembers() {

this.memberService.getMembers(this.pageNumber, this.pageSize).subscribe({

next: (response) => {

this.paginatedMembers.set(response);

},

});

}

  

onPageChange(pageNumber: number, pageSize: number) {

this.pageNumber = pageNumber;

this.pageSize = pageSize;

this.loadMembers();

}

  

ngOnInit() {

this.memberService.member.set(null);

this.loadMembers();

}

}

@if(paginatedMembers(); as paginatedResult) { @if(paginatedResult.metadata; as metadata) {

<app-paginator

[pageNumber]="metadata.currentPage"

[pageSize]="metadata.pageSize"

[totalCount]="metadata.totalCount"

[totalPages]="metadata.totalPages"

(pageChange)="onPageChange($event.pageNumber, $event.pageSize)"

></app-paginator>

} @if(paginatedResult.items; as members) {

<div class="grid grid-cols-5 gap-6">

@for (member of members; track member.id) {

<app-member-card [member]="member"></app-member-card>

}

</div>

} }
```