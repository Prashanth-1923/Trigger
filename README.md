trigger AccountContact on Account (after insert,after update) {

   account acc =[select id,outOfZip__c,BillingPostalCode from account where id in : trigger.new];
   List<contact>conList = [select id, name,MailingPostalCode from contact where accountId in : acc];
   List<contact>conListNew = new List<contact>();
   for (contact con : conList)
    {
   if(acc.BillingPostalCode != con.MailingPostalCode)
    {
      conListNew.add(con);
    } 

   If(conListNew.size()>=1)
   {
  acc.outOfZip__c=true;
   }
   update acc;
   }
  }
