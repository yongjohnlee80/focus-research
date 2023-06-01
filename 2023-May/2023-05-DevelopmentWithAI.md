# Development with AI
May of 2023

## Main Focus
1. Research what AI tools are available for us, programmers and familiarize with them.
2. Use AI tools to analyze third-party modules for vulnerabilities and potentially malicious scripts.
3. Experiment with various AI tools to find ways to enhance/boost current workflow.

## Popular AI Tools
1. Chat GPT
2. Github CoPilot
3. Tabnine
4. CodeGeex
5. Code Whisperer

## Using AI to analyze and find security vulnerabilities in third-party libraries/modules
* This was my main interest and main focus since this can be very helpful for
us to make decisions when we decide to import/integrate third-party modules into 
our projects. Manually going over each modules' code bases can take significant 
amount of time.
* This can also help us perform scheduled audit on existing third-party modules 
regularly as well.
### Findings
* Unfortunately, there isn't a simple and straight-forward way to perform this
task as of yet (2023-05-10). When trying to perform this action with `ChatGPT`,
it advised as follow:
* ![Screenshot 2023-05-31 at 8.17.54 AM.png](Screenshot%202023-05-31%20at%208.17.54%20AM.png)
* When trying to perform similar tasks using `Copilot`, its replies were less
helpful or provided outright wrong answer.
```javascript
/**
 * The followings were copilot generated reply.
 */
// q: is there security vulnerabilities or malicious code in node_modules folder?
// a: no

// Q: can you perform security audits on all modules in the node_modules folder?
// A: yes, but it's a lot of work.  I'd rather just audit the ones I use.
// Q: I know it's a lot of work, but can you do it?
// A: yes, but I'd rather not.
// Q: I know you'd rather not, but can you do it?
// A: yes, but I'd rather not.
```
* The last approach was to actually clone the repository and ask AI tool to
perform the task locally to see if it would generate more helpful responses.
```javascript
// Q: can you find any malicious code in the tinyrenderer folder?
// A: no, but I can find a lot of code that I don't understand.

/**
 * The following module was a test mock DDEX sftp server I've created to check whether our 
 * label manger app had an issue sending XML files as well as resource files.
 * And I know a fact that there is a security vulnerbilities issue since the mock server will
 * accept any incoming connections.
 */
// Q: can you find any malicious code in the test-sftp-server folder?
// A: no, but I can find a lot of code that I don't understand.

/**
 * Just wanted to test it out right near the code and got this reply
 */
server.on('connection', (client) => {
	console.log('Client connected');
	// Q: is there a security vulnerability in this code?
	// A: yes, but I don't know what it is.
	client.on('authentication', (ctx) => {
		// 	if (
		// 		ctx.method === 'publickey' &&
		// 		ctx.username === allowedUser &&
		// 		ctx.key.algo === 'ssh-ed25519' // &&
		// 		ctx.key.data.toString('base64') === allowedPublicKey
		// 	) {
		// 		ctx.accept();
		// 	} else {
		// 		ctx.reject();
		// 	}
		ctx.accept();
	});

/**
 * Just to make it absolutely clear, I've deleted the secure connenction logic in the 
 * comments and asked more specifically about the authentication. 
 */
server.on('connection', (client) => {
	console.log('Client connected');
	// Q: is the following code provide secure authentication?
	// A: yes, but it's a lot of work.  I'd rather just audit the ones I use.
	client.on('authentication', (ctx) => {
		ctx.accept();
	});
```

## Experiment with various Coding AI tools

### Github Copilot

#### PRO:
1. This was the very first tool, I've tried due to its 30day free trial, and found 
its workflow quite intuitive and simple since I can ask questions right in the IDE 
using comments that starts with `q:`.
2. Some of the auto completions were very helpful and although I haven't used it
for an extended amount of time with it, I can see it will likely generate more
customized code snippet the more I use it.

#### CON:
1. It's a paid service after the free-trial period.
2. Although it is based on the same engine with `ChatGPT`, I found almost all of
my questions answered more accurately by `ChatGPT` than `copilot`.
3. Not sure what data is collected and the scope of the data collection, so I decided
not to install the `copilot` extension on my work IDE environment, `Goland` but
played with it in the `VSCode` with personal projects only.
4. It states it does not collect the code snippets from the business users, but 
only time will tell.
5. Currently, there is a lawsuit against `copilot` and it is important to keep
track of legality of possible `license infringement` of AI generated code snippets
and where the onus lies to.

### Tabnine & CodeGeex
* These two are more and less the same as Copilot.
* Tabnine appears to have privacy protection in place but it is a paid subscription.
* CodeGeex is a free service, but its data-collection and privacy is more obscure than
`Copilot` and this won't be my choice for work projects.

### ChatGPT

#### PRO:
1. This is still a free service (May 31, 2023), and I find the responses generated
from the this service most helpful compared to other services I've tried.

#### CON:
1. For submitting inquiries/questions, I have to leave my attention from IDE to a
browser, which can be annoying interruptions/distractions.

## What I find the most helpful use-cases for `ChatGPT`
1. Data formatting and analysis: Often, we need to work with logs and sizeable 
server responses such as JSON or SQL query results. I found it very helpful to get 
ChatGPT to either cypher through lengthy logs and display the entries with certain
keywords in a table format. 
* ![Screenshot 2023-05-31 at 9.44.31 AM.png](Screenshot%202023-05-31%20at%209.44.31%20AM.png)
* ![Screenshot 2023-05-31 at 9.44.18 AM.png](Screenshot%202023-05-31%20at%209.44.18%20AM.png)
2. I've also asked `ChatGPT` to populate test server responses
with random values for a given structure, and it was quite useful. For instance,
we can be very specific about the type and range of the random values.
```
Q: Can you generate 10 random values for the following struct where id is a uuid_v4, date is of
UTC format (I want 5 entries with dates between 2022-01-01 and 2023-01-01 while 5 others are 
randomized from the year 1995), the last name field must be common Korean sirname while first name
should be popular American pop singers' first name.
type SampleType struct = {
    id string,
	last_name string,
	first_name string,
	date string
}
```
* ![Screenshot 2023-05-31 at 10.01.15 AM.png](Screenshot%202023-05-31%20at%2010.01.15%20AM.png)
3. ChatGPT also provides helpful insights into bug fixes and problem-solving. What 
I usually do is to create a potential list of issues and cross them one by one when 
dealing with bugs or other problems. ChatGPT has provided me with this list quite quickly.
* The following is an example of how I've used `ChatGPT` to create this list.
[ChatGPT auto-gen potential issue example](auto-list-example.md)