Creating and Running Unit Tests for Managed Code
------------------------------------------------

**Issues during working process on VS**

1.If project requires to test a local software, we just need to create a class library project which can not be executed.

2.Add a Unit Test project inside of the solution and set it as StartUp project.

3.Write source codes into class library, write test cases into unit test project.

**Prepare the walkthrough**

1.Open Visual Studio.
2.On the File menu, point to New and then click Project.
3.The New Project dialog box appears.
4.Under Installed Templates, click Visual C#.
5.In the list of application types, click Class Library.
6.In the Name box, type Bank and then click OK.

    // method under test
    public void Debit(double amount)
    {
        if(amount > m_balance)
        {
            throw new ArgumentOutOfRangeException("amount");
        }
        if (amount < 0)
        {
            throw new ArgumentOutOfRangeException("amount");
        }
        m_balance += amount;
    }
**Create a unit test project**

1.On the File menu, choose Add, and then choose New Project ....
2.In the New Project dialog box, expand Installed, expand Visual C#, and then choose Test.
3.From the list of templates, select Unit Test Project.
4.In the Name box, enter BankTest, and then choose OK.
5.The BankTests project is added to the the Bank solution.
6.In the BankTests project, add a reference to the Bank solution.
7.In Solution Explorer, select References in the BankTests project and then 8.choose Add Reference... from the context menu.
9.In the Reference Manager dialog box, expand Solution and then check the Bank item.

    // unit test code
    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    
    namespace BankTests
    {
        [TestClass]
        public class BankAccountTests
        {
            [TestMethod]
            public void TestMethod1()
            {
            }
        }
    }

**Create the first test method**

    // unit test code
    [TestMethod]
    public void Debit_WithValidAmount_UpdatesBalance()
    {
        // arrange
        double beginningBalance = 11.99;
        double debitAmount = 4.55;
        double expected = 7.44;
        BankAccount account = new BankAccount("Mr. Bryan Walton", beginningBalance);
    
        // act
        account.Debit(debitAmount);
    
        // assert
        double actual = account.Balance;
        Assert.AreEqual(expected, actual, 0.001, "Account not debited correctly");
    }

**Create the second test method**

    //unit test method
    [TestMethod]
    [ExpectedException(typeof(ArgumentOutOfRangeException))]
    public void Debit_WhenAmountIsLessThanZero_ShouldThrowArgumentOutOfRange()
    {
        // arrange
        double beginningBalance = 11.99;
        double debitAmount = -100.00;
        BankAccount account = new BankAccount("Mr. Bryan Walton", beginningBalance);
    
        // act
        account.Debit(debitAmount);
    
        // assert is handled by ExpectedException
    }
