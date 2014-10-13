create trigger [dbo].[td_customer] on [dbo].[customer] for delete as
begin
    declare
       @numrows  int,
       @errno    int,
       @errmsg   varchar(255)

    select  @numrows = @@rowcount
    if @numrows = 0
       return

    
    if exists (select 1
               from   service t2, deleted t1
               where  t2.customer_id = t1.customer_id)
       begin
          select @errno  = 50006,
                 @errmsg = 'Children still exist in "service". Cannot delete parent "customer".'
          goto error
       end

    return
    
error:
	raiserror ('%s %i: %s', 16, 1, 'Error in trigger td_customer', @errno, @errmsg)
    rollback  transaction
end
GO
