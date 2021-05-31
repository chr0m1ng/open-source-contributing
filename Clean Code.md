# 🔡 Clean Code

> "Clean code always looks like it was written by someone who cares" - Robert C. Martin

## Contents

- [Motivations](#motivations)
- [Concepts](#concepts)
  - [Maintainability](#maintainability)
  - [Code Smell](#code-smell)
  - [Clean Code](#clean-code)
- [Legibility](#legibility)
  - [Names matter](#names-matter)
- [Comments](#comments)
- [Formatting](#formatting)
- [References](#References)

## Motivations

> "You are reading this book for two reasons. First, you are a programmer. Second, you want to be a better programmer. Good. We need better programmers" - Robert C. Martin (**Clean Code**)

- Maintenance costs can reach up to 90% of the total development budget;
- You are part of a team! Your code should be easy to read not only for you, but for others.

---

## Concepts

### Maintainability

- How easy it is to maintain a system;
- Hard to quantify;
- Might be delimited by the systems' characteristics.

### Code Smell

> "Smells are certain structures in the code that indicate violation of fundamental design principles and negatively impact design quality" - Suryanarayana, Girish (**Refactoring for Software Design Smells**)

- Not always bugs!
  - they aren't technically incorrect
  - the program still functions as intended
- Weakness in design that might slow down development and even increase the risk of bugs or **failures** in the future

### Clean Code

> “Clean code is simple and direct. Clean code reads like well-written
prose. Clean code never obscures the designer’s intent but rather is
full of abstractions and straightforward lines of control.” - Grady Booch (**UML**)

---

> “I like my code to be elegant and efficient. The logic should be
straightforward to make it hard for bugs to hide, the dependencies
minimal to ease maintenance, error handling complete according
to an articulated strategy, and performance close to optimal so as
not to tempt people to make the code messy with unprincipled
optimizations. Clean code does one thing well.” - Bjarne Stroustrup (**C++**)

---

> “Clean code can be read, and enhanced by a developer other than
its original author. It has unit and acceptance tests. It has meaningful
names. It provides one way rather than many ways for doing one
thing. It has minimal dependencies, which are explicitly defined, and
provides a clear and minimal API.” - Dave Thomas (**Eclipse**)

---

## Legibility

### Names matter

- Use names that reveal intent:

Do:

```csharp
public List<Cell> GetFlaggedCells()
{
    var flaggedCells = new List<Cell>();
    for(var cell in gameBoard)
    {
        if(cell.IsFlagged())
        {
            flaggedCells.Add(cell);
        }
    }
    return flaggedCells;
}
```

Don't:

```csharp
public List<int[]> GetThem()
{
    var newList = new List<int>();
    for(var c in list)
    {
        if(c[0] == 4)
        {
            newList.Add(c);
        }
    }
    return newList;
}
```

- Distinct between names:

Do:

```csharp
public static void CopyCharArray(char[] source, char[] destination)
{
    for(int index = 0; index < source.Length; index++)
    {
        destination[index] = source[index];
    }
}
```

Don't:

```csharp
public static void CopyCharArray(char[] a1, char[] a2)
{
    for(int i = 0; i < a1.Length; i++)
    {
        a2[i] = a1[i];
    }
}
```

- Use names you can pronounce:

Do:

```csharp
public class Customer
{
    public DateTime creationTimestamp { get; set; };
    public DateTime lastEditTimestamp { get; set; };
    public int customerId { get; set; };
}
```

Don't:

```csharp
public class Customer
{
    public DateTime crtTmstmp { get; set; };
    public DateTime edtTimstmp { get; set; };
    public int cstId { get; set; };
}
```

- Use searchable names:

Do:

```csharp
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for(int index = 0; index < NUMBER_OF_TASKS; index++)
{
    int realTaskDays = taskEstimate[index] * realDaysPerIdealDay;
    int realTaskWeeks = realTaskDays / WORK_DAYS_PER_WEEK;
    sum += realTaskWeeks;
}
```

Don't:

```csharp
for(int j=0; j<34; j++)
{
    s += (t[j]*4)/5;
}
```

---

## Comments

- Nothing is more useful than a good comment!
- But nothing is worse than a bad, redundant or unnecessary comment/documentation
- Comments are for when we fail to express something with code
  - Do not confuse comments with documentation!

Do:

> Note that the code itself tells you what it's doing

```csharp
if(employee.IsEligibleForFullBenefits())
```

Don't:

```csharp
// Checks if employee has everything they need to be eligible for benefits
if((employee.Flags & HOURLY_FLAG) && (employee.Age > 65))
```

Do:

```csharp
public class Student
{
    public int Age { get; set; }
    public string Name { get; set; }
    public List<Subject> Subjects { get; set; }

    public Student()
    {
        Subjects = new List<Subject>();
    }
}
```

Don't:

```csharp
public class Student
{
    /// <summary>
    /// Student's Age
    /// </summary>
    public int Age { get; set; }

    /// <summary>
    /// Student's Name
    /// </summary>
    public string Name { get; set; }

    /// <summary>
    /// Student's Subjects
    /// </summary>
    public List<Subject> Subjects { get; set; }

    /// <summary>
    /// Default ctor
    /// </summary>
    public Student()
    {
        Subjects = new List<Subject>();
    }
}
```

- Commented-out code *should not* be commited!
Do:

```csharp
var studentNameList = subject.Students(s => s.Name).ToList();
```

Don't:

```csharp
// var studentNameList = new List<string>();
// foreach(var student in subject.Students)
// {
//     studentNameList.Add(student.Name);
// }
var studentNameList = subject.Students(s => s.Name).ToList();
```

---

## Formatting

- How long should a file be? (Vertical formatting)
If your class/interface follows the **Single Responsibility Principle**, it should not be too long. If it is, it can probably be refactored so that it is shortened.

Do:

![image.png](https://i.imgur.com/x2ZQnSr.png)

Don't:

![image.png](https://i.imgur.com/L8h22r4.png)

- Every block of lines should represent a concept

Do:

```csharp
if (settings.Headers != null)
{
    foreach (var header in settings.Headers)
    {
        httpRequestMessage.Headers.TryAddWithoutValidation(header.Key, header.Value);
    }
}

if (!string.IsNullOrWhiteSpace(settings.Body))
{
    string contentType = null;
    settings.Headers?.TryGetValue("Content-Type", out contentType);
    httpRequestMessage.Content = new StringContent(settings.Body, Encoding.UTF8,
        contentType ?? "application/json");
}

AddUserToHeaders(httpRequestMessage, context);
AddBotIdentityToHeaders(httpRequestMessage, context);
```

Don't:

```csharp
if (settings.Headers != null)
{
    foreach (var header in settings.Headers)
    {
        httpRequestMessage.Headers.TryAddWithoutValidation(header.Key, header.Value);
    }
}
if (!string.IsNullOrWhiteSpace(settings.Body))
{
    string contentType = null;
    settings.Headers?.TryGetValue("Content-Type", out contentType);
    httpRequestMessage.Content = new StringContent(settings.Body, Encoding.UTF8, contentType ?? "application/json");
}
AddUserToHeaders(httpRequestMessage, context);
AddBotIdentityToHeaders(httpRequestMessage, context);
```

- How long should a line be? (Horizontal formatting)

There isn't a general rule of how many columns your line can have, but avoid creating lines that go too far. Specially if you need to **scroll horizontally**. There is a [whole article about this](https://en.wikipedia.org/wiki/Characters_per_line) (and an entire internet discussion as well, for example [here](https://nickjanetakis.com/blog/80-characters-per-line-is-a-standard-worth-sticking-to-even-today)).

Do:

```csharp
var suffix = origin == ExceptionOrigin.Bot
           ? _errorMessages.Bot[messageCode.ToString()]
           : _errorMessages.Digio[messageCode.ToString()];
```

Don't:

```csharp
var suffix = origin == ExceptionOrigin.Bot ? _errorMessages.Bot[messageCode.ToString()] : _errorMessages.Digio[messageCode.ToString()];
```

- How long should a method be?

Avoid writing very long methods. A method usually has between 1 and 25 lines of code. If a method has more than 25 lines, consider [refactoring](https://refactoring.com/catalog/) parts of it into different methods. Remember methods should do *one* job only!
Do:

```csharp
public async Task<IActionResult> GetInsuranceQuantity(ApiRequestBase request)
{
    try
    {
        _logger.Information("[AzulFinanceiro] Trying to recover policy quantity for user {@identity}", request.Identity);
        var from = request.Identity;
        IEnumerable<InsurancePolicy> insurances = null;
        var quantity = string.Empty;

        var userContext = await _contextManager.GetUserContextAsync(from, cancellationToken);
        var insurances = await GetInsurancesAsync(userContext);
        var insurancesCount = insurances.Count();
        quantity = CheckQuantity(insurancesCount).ToString().ToLower();

        var response = new BaseResponse
        {
            Text = quantity
        };

        await UpdateContextFlowAsync(userContext, request);

        _logger.Information("[AzulFinanceiro] Sucessfully recovered {@quantity} policies for user {@identity}", quantity, request.Identity);
        return Ok(response);
    }
    catch (Exception ex)
    {
        _logger.Error(ex, "[AzulFinanceiro] Failed to get insurance quantity for user {@identity}. Original message ID: {@messageId}", request.Identity, request.MessageId);
        return BadRequest(ex);
    }
}
```

Don't:

```csharp
public async Task<IActionResult> GetInsuranceQuantity(ApiRequestBase request)
{
    try
    {
        _logger.Information("[AzulFinanceiro] Trying to recover policy quantity for user {@identity}", request.Identity);
        var from = request.Identity;
        IEnumerable<InsurancePolicy> insurances = null;
        var quantity = string.Empty;

        var userContext = await _contextManager.GetUserContextAsync(from, cancellationToken);
        try
        {
            userContext.UserFlow = Enum.Parse<Flow>(request.ExtraInfo);
        }
        catch (ArgumentException aex)
        {
            _logger.Warning(aex, "Tried to parse {@attemptedParse} into {@enum} for user {@identifier}", request.ExtraInfo, nameof(Flow), from);
            return StatusCode(412, aex);
        }

        if (userContext.UserType.ToLower() == UserType.Insured.ToString().ToLower())
        {
            insurances = await _insurancePolicyService.GetInsurancesByIdentifierAsync(userContext.UserIdentifier);
            userContext.CreateInsurancePolicies(insurances);
        }
        else
        {
            insurances = userContext.InsurancePolicies;
        }

        var insurancesCount = insurances.Count();

        if (insurancesCount == 1)
            userContext.SelectedInsurancePolicy = insurances.First().PolicyIdentifier;

        await _contextManager.SetUserContextAsync(from, userContext, cancellationToken);
        quantity = CheckQuantity(insurancesCount).ToString().ToLower();

        var response = new BaseResponse
        {
            Text = quantity
        };
        _logger.Information("[AzulFinanceiro] Sucessfully recovered {@quantity} policies for user {@identity}", quantity, request.Identity);
        return Ok(response);
    }
    catch (Exception ex)
    {
        _logger.Error(ex, "[AzulFinanceiro] Failed to get insurance quantity for user {@identity}. Original message ID: {@messageId}", request.Identity, request.MessageId);
        return BadRequest(ex);
    }
}
```

- Single Responsibility Principle (The **S** from S.O.L.I.D.)

Classes (and methods!) should have a single purpose, scope, responsibility, or whatever you wish to call it. If you have more than one responsibility per class/method, you will end up having to change it frequently, which causes the class/method to be highly unstable, which in turn makes it **even more important** to have unit tests for your methods, which **is not simple** if your method does too much, and from here on it gets only worse!

Do:

```csharp
public interface ISchedulerService
{
    Task<bool> ScheduleAtendence(string identifier, string contactIdentity, string name, string msgId, string channel);

    Task<bool> ScheduleGettingOut(string identifier, string contactIdentity, string name);

    Task<bool> ScheduleProposalsAbandon(GeneralIdentifier identifiers, string contactIdentity, Session session, string message, int minutes);

    Task<bool> CancelScheduleGettingOut(string identifier, string contactIdentity);

    Task<bool> CancelScheduleProposalsAbandon(string identifier, string contactIdentity, IDictionary<string, string> extras);

    Task<InstallmentsNotificationsScheduled> CancelInstallmentsNotification(string identifier, InstallmentsNotificationsScheduled notifications);

    Task<bool> ScheduleWhatsAppNotification(string schedulerId, string resourceKey, string contactIdentity);

    Task<bool> CancelScheduledMessage(string schedulerId);
}
```

Don't:

```csharp
public interface IGeneralService
{
    Task<Customer> GetCustomerInformationAsync(string identifier, Domain.Contract.Session session);

    Task<List<Debt>> GetDebtsAsync(string identifier, Domain.Contract.Session session);

    Task<bool> HasDiscount(GeneralIdentifier identifiers, Domain.Contract.Session session);

    Task<DocumentCollection> GetProposalCarouselAsync(GeneralIdentifier identifiers, Domain.Contract.Session session, bool isOthers);

    Task<DocumentCollection> GetDebtsCarouselAsync(string identifier, Domain.Contract.Session session);

    Task<DocumentCollection> GetDebtsDocumentCollectionAsync(string identifier, Domain.Contract.Session session);

    Task<string> GetDebtsListAsync(string identifier, Domain.Contract.Session session);

    Task<PlainText> GetDebtsSubProducts(string identifier, Domain.Contract.Session session);

    Task<Select> GetPaymentDaysQuickReplyAsync(PaymentMethodIdentifier identifiers, Domain.Contract.Session session);

    Task<DocumentCollection> GetPaymentDaysDocumentCollectionAsync(PaymentMethodIdentifier pmIdentifiers, Domain.Contract.Session session);

    Task<Document> GetPaymentPDFMessage(Domain.Contract.Session session, int agreementId, int installment, int month, string wallet);

    Task<string> GetMaxDiscountAmountAsync(GeneralIdentifier identifiers, Domain.Contract.Session session);

    Task<Proposal> GetProposalsAsync(GeneralIdentifier prop, Domain.Contract.Session session, decimal amountSuggested = decimal.Zero, bool inCash = false);

    Task<DocumentCollection> GetProposalDocumentCollectionAsync(GeneralIdentifier identifiers, Domain.Contract.Session session, bool isOthers, int amount = 0, bool inCash = false);

    Task<PlainText> GetProposalPlainTextAsync(GeneralIdentifier identifiers, Domain.Contract.Session session, bool isOthers, int amount = 0, bool inCash = false);

    Task<Debt> GetLowestDebtAmount(string identifier, Domain.Contract.Session session);

    Task<int> ConfirmAgreement(GeneralIdentifier prop, long formaPagamento, string diaVencimento, Domain.Contract.Session session);

    DocumentCollection GetAgreementResumeDocumentCollection(GeneralIdentifier identifiers, PlainText agreementResume);

    Task<bool> SendEmail(GeneralIdentifier prop, int stallment, int agreementId, Domain.Contract.Session session);

    Task<bool> SetEmail(string identifier, string email, Domain.Contract.Session session);

    Task<string> GetPaymentLine(GeneralIdentifier prop, int stallment, int agreementId, Domain.Contract.Session session);

    Task<bool> ScheduleAtendence(string identifier, string contactIdentity, string name, string msgId, string channel);

    Task<bool> ScheduleGettingOut(string identifier, string contactIdentity, string name);

    Task<bool> TrackScheduledMessages(string msg);

    Task<bool> CancelScheduleGettingOut(string identifier, string contactIdentity);

    Task<List<Agreement>> GetAgreements(Domain.Contract.Session session);

    Task<List<Agreement>> GetValidAgreements(Domain.Contract.Session session);

    Task<PlainText> GetValidAgreementsPlainText(Domain.Contract.Session session);

    Task<Document> GetPDFPaymentCopy(Domain.Contract.Session session, int agreementId, string wallet);

    Task<List<Takenet.Iris.Messaging.Resources.ThreadMessage>> GetMessagesHistory(string contactId);

    Task<bool> MonitoreReceivedProposalNotificationAsync(string identifier, string contactId, string notificationSchedulerId, IDictionary<string, string> extras);

    Task<bool> MonitoreReceivedAgreementNotificationAsync(string identifier, string contactIdentity, string schedulerId, IDictionary<string, string> extras);

    Task<bool> MonitoreReceivedCPFNotificationAsync(string identifier, string contactIdentity, string schedulerId, IDictionary<string, string> extras);

    Task<RecoveryWhatsWS.LoteLibreDeuda> GetDischargeLetter(Domain.Contract.Session session);

    Task<List<MediaLink>> GetPdfDischargeLetter(Domain.Contract.Session session, RecoveryWhatsWS.LoteLibreDeuda loteLibraDeuda);
}
```

or in a less extreme case

Do:

```csharp
public class Version
{
    public int GetMajorVersionNumber();
    public int GetMinorVersionNumber();
    public int GetBuildNumber();
}
```

Don't:

```csharp
public class SuperDashboard
{
    public Component GetLastFocusedComponent();
    public void SetLastFocused(Component lastFocused);
    public int GetMajorVersionNumber();
    public int GetMinorVersionNumber();
    public int GetBuildNumber();
}
```

---

## References

- "Clean Code", a book by Robert C. Martin (aka Uncle Bob)
  - For those who want a _pocket_ version, check this [summarized article](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29)
- "Refactoring", a book by Martin Fowler
- [This catalog of refactorings based on Fowler's book](https://refactoring.com/catalog/)
- [This friendly list of examples of code smells](https://sourcemaking.com/refactoring/smells)
