MiniZip
=======

New MiniZip without warning compile at xCODE 4 iOS SDK 6.1

How To Add into Your Project
============================

1. Drag and Drop into xcode your project

2. Note: No ARC Support

	If you are using an ARC enabled project, you will need to tell the compiler not to use ARC for ZipArchive. To do this, click on the project in the left hand column. Then click on your target in the middle column and select the “Build Phases” tab.

	Expand the “Compile Sources” area, locate ZipArchive.mm and double click on it. In the box that pops up, type in -fno-objc-arc and click Done.

3. Linking libz

	The last step is to link your project against libz.dylib. From the Build Phases screen you navigated to above, expand the Link Binary With Libraries section and click the “+” button to add a new library. Search the list for libz.1.2.5.dylib, select it and click Add.

How To Use
==========

	#import "ZipArchive.h" 

	/*at some method*/
	ZipArchive* za = [[ZipArchive alloc] init];
	if( [za UnzipOpenFile:[[NSBundle mainBundle] pathForResource:@"masbog" ofType:@"zip"]] )
	{
	
		NSString *strPath=[NSString stringWithFormat:@"%@/UnzippedFolder",[self getDocumentFileDirectory]];
		//Delete all the previous files
		NSFileManager *filemanager=[[NSFileManager alloc] init];
		if ([filemanager fileExistsAtPath:strPath]) 
		{
			//Check if file exist	
			NSError *error;
			[filemanager removeItemAtPath:strPath error:&error];
		}
	
		filemanager=nil;
		//start unzip
		BOOL ret = [za UnzipFileTo:[NSString stringWithFormat:@"%@/",strPath] overWrite:YES];
		if( NO==ret ){
			// error handler here
			UIAlertView *alert=[[UIAlertView alloc] initWithTitle:@"Error"
														  message:@"An unknown error occured"
														 delegate:self
												cancelButtonTitle:@"OK"
												otherButtonTitles:nil];
			[alert show];
			[alert release];
			alert=nil;
		}
		[za UnzipCloseFile];
	}			
