**CHECK SHAPING RETURNS UNDER CONTROLLERS FOLDER AS WELL

Adding what's called a navigation property will let EF know to build a relationship. We've got 3 entities: a User, which is basic user info. A member, which is the detailed info about the user that he uploads for the public. The photo, which are photos that users can upload and share. In each of these, we've got a reference to the other type:

1. In user we got:
   public Member Member { get; set; } = null!;
2. In member we got:
   [ForeignKey(nameof(Id))]
   public AppUser AppUser { get; set; } = null!;
 .     public List<Photo> Photos { get; set; } = [];
   
   So that we tell EF what the foreign key of member is within app user class
3. In photo we got:
   public Member Member { get; set; } = null!;

This will tell ef to build a relationship between the tables it generates in the exact way we set it up in the entities classes.

This would result in something like being generated:

`migrationBuilder.AddColumn<string>(`

`name: "ImageUrl",`

`table: "Users",`

`type: "TEXT",`

`nullable: true);`

  

`migrationBuilder.CreateTable(`

`name: "Members",`

`columns: table => new`

`{`

`Id = table.Column<string>(type: "TEXT", nullable: false),`

`DateOfBirth = table.Column<DateOnly>(type: "TEXT", nullable: false),`

`ImageUrl = table.Column<string>(type: "TEXT", nullable: true),`

`DisplayName = table.Column<string>(type: "TEXT", nullable: false),`

`Created = table.Column<DateTime>(type: "TEXT", nullable: false),`

`LastActive = table.Column<DateTime>(type: "TEXT", nullable: false),`

`Gender = table.Column<string>(type: "TEXT", nullable: false),`

`Description = table.Column<string>(type: "TEXT", nullable: true),`

`City = table.Column<string>(type: "TEXT", nullable: false),`

`Country = table.Column<string>(type: "TEXT", nullable: false)`

`},`

`constraints: table =>`

`{`

`table.PrimaryKey("PK_Members", x => x.Id);`

`table.ForeignKey(`

`name: "FK_Members_Users_Id",`

`column: x => x.Id,`

`principalTable: "Users",`

`principalColumn: "Id",`

`onDelete: ReferentialAction.Cascade);`

`});`

  

`migrationBuilder.CreateTable(`

`name: "Photos",`

`columns: table => new`

`{`

`Id = table.Column<int>(type: "INTEGER", nullable: false)`

`.Annotation("Sqlite:Autoincrement", true),`

`PublicId = table.Column<string>(type: "TEXT", nullable: false),`

`Url = table.Column<string>(type: "TEXT", nullable: false),`

`MemberId = table.Column<string>(type: "TEXT", nullable: false)`

`},`

`constraints: table =>`

`{`

`table.PrimaryKey("PK_Photos", x => x.Id);`

`table.ForeignKey(`

`name: "FK_Photos_Members_MemberId",`

`column: x => x.MemberId,`

`principalTable: "Members",`

`principalColumn: "Id",`

`onDelete: ReferentialAction.Cascade);`

`});`

  

`migrationBuilder.CreateIndex(`

`name: "IX_Photos_MemberId",`

`table: "Photos",`

`column: "MemberId");`

