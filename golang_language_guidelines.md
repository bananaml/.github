# Dev vs Prod Flags
In a go package we use the init() function to define environment variables

This makes it easy to switch between dev and prod, keep track of env var changes, and works with any dev flow

your init() function should handle 3 cases as follows
```
func init() 
    if os.Getenv("ENV) == "prod" {
        // set environment variables via either secret manager or hard coded here
        os.Setenv("some var", "some value)...
        thirdparty.secretmanager.getVar("some secret var", "some secret value")

        // All env vars should be accessible via os.getenv("var name")
    } else if os.Getenv("ENV") == "dev" {
        // same deal as above, except use dev values for staging databases etc
    } else {
        // enforce ENV is set and valid or else quit
		panic("ENV must be set to prod or dev")
    }
}
```

Then access them in your program via os.Getenv("your var...")
