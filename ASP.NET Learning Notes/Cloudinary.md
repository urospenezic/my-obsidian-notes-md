
a free way to store photos on cloud (up to 10GB). 
1. To set it app add to appsettings.json:
   "CloudinarySettings": {
   "CloudName": "name",
   "ApiKey": "123456789012345",
   "ApiSecret": "REPLACE_WITH_PRODUCTION_API_SECRET"
   },
   do not be an idiot and push appsettings.json to github public repos :)
2. Hook it up in Program.cs:
   builder.Services.Configure<API.Helpers.CloudinarySettings>(
   builder.Configuration.GetSection("CloudinarySettings")
	);
	CloudinarySettings class is just for type safety, names of props match the names of json props
3. Add NuGet package for cloudinary: CloudinaryDotnet
4. Create a service (example below)

NOTE: ImageResult returns a public id property, which is why we've added PublicId and Id to our entity. IFormFile is aspnet lib for file management. Gravity("face") inside of transormation is to add focus to the image if its of persons face (probably AI filters)

`public class PhotoService : IPhotoService`
`{`
	`private readonly Cloudinary _cloudinary;`
	`public PhotoService(IOptions<CloudinarySettings> config)`
	`{`
		`var cloudinaryConfig = new Account(`
		`config.Value.CloudName,`
		`config.Value.ApiKey,`
		`config.Value.ApiSecret );`

		`_cloudinary = new Cloudinary(cloudinaryConfig);`
	`}`

	`public async Task<ImageUploadResult> AddPhotoAsync(IFormFile file)`
	`{`
		`var uploadResult = new ImageUploadResult();`
		`//if the file exists and has content`
		`if (file.Length > 0)`
		`{`
			`await using var stream = file.OpenReadStream();`
			`var uploadParams = new ImageUploadParams {`
				`File = new FileDescription(file.FileName, stream),`
				`Transformation = new Transformation()`
					`.Height(500)`
					`.Width(500)`
					`.Crop("fill")`
					`.Gravity("face"),`
					`Folder = "datingapp",`
				`};`
			`uploadResult = await _cloudinary.UploadAsync(uploadParams);`
		`}`
		`return uploadResult;`
	`}`

	`public async Task<DeletionResult> DeletePhotoAsync(string publicId)`
	`{`
		`var deletionParams = new DeletionParams(publicId);`
		`return await _cloudinary.DestroyAsync(deletionParams);`
	`}`
`}`


