# Find binary and dynamic so library dependence belongs to which RPM

When we build RPM, we should configure the dependence package manually. It's easy to miss some package. So I write this tools to find the dependence package.

## Usage

find all RPM deps with missing info

	# finddeprpm /opt/f8n/nginx 
	krb5-libs	1.12.2
	libcom_err	1.42.9
	xz-libs	5.1.2
	libjpeg-turbo	1.2.90
	keyutils-libs	1.5.8
	libXau	1.0.8
	pcre	8.32
	libxslt	1.1.28
	...

just find RPM deps existed without missing info

	# finddeprpm /opt/f8n/nginx 2> /dev/null

find missing library

	# finddeprpm /opt/f8n/nginx 1> /dev/null

###How to reference finddeprpm as lib in other go program

	...

	import (
		finddeprpm "github.com/cst05001/finddeprpm/lib"
	)

	...

	func main() {
		...
		rpmList, missingLDList := lib.FindDepRPM(path)
		for _, rpm := range(rpmList) {
			fmt.Printf("%s\t%s\n", rpm.Name, rpm.Version)
		}
		for _, ld := range(missingLDList) {
			fmt.Fprintf(os.Stderr, "%s%s%s\n", colorStart, ld, colorEnd)
		}
		...
	}

