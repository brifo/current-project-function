package com.olegburmistrov.jira;

import com.atlassian.jira.plugin.jql.function.AbstractJqlFunction;
import com.atlassian.jira.util.MessageSet;
import com.atlassian.jira.JiraDataType;
import com.atlassian.jira.JiraDataTypes;
import com.atlassian.jira.jql.operand.QueryLiteral;
import com.atlassian.jira.jql.query.QueryCreationContext;
import com.atlassian.jira.jql.validator.NumberOfArgumentsValidator;
import com.atlassian.jira.security.Permissions;
import com.atlassian.query.clause.TerminalClause;
import com.atlassian.query.operand.FunctionOperand;
import com.atlassian.jira.user.UserProjectHistoryManager;
import com.opensymphony.user.User;
import java.util.List;
import java.util.Collections;

/**
 * CurrentProjectFunction implementation by 
 * @author Oleg Burmistrov
 * 
 */
public class CurrentProjectFunction extends AbstractJqlFunction
{
	private final UserProjectHistoryManager userProjectHistoryManager;
	
	public CurrentProjectFunction(UserProjectHistoryManager userProjectHistoryManager)
	{
		this.userProjectHistoryManager = userProjectHistoryManager;
	}
	
	public String getFunctionName()
	{
		return "currentProject";
	}
	
	private static final int EXPECTED_ARGS = 0;

    public MessageSet validate(User searcher, FunctionOperand operand, final TerminalClause terminalClause)
    {
        return validateNumberOfArgs(operand, EXPECTED_ARGS);
    }

    public List<QueryLiteral> getValues(QueryCreationContext queryCreationContext, FunctionOperand operand, final TerminalClause terminalClause)
    {
        if (queryCreationContext == null || queryCreationContext.getUser() == null)
        {
            return Collections.emptyList();
        }
        
        if (userProjectHistoryManager.getCurrentProject(Permissions.BROWSE, queryCreationContext.getUser()) == null)
        {
        	return Collections.emptyList();
        }
        else
        {
        	return Collections.singletonList(new QueryLiteral(operand, userProjectHistoryManager.getCurrentProject(Permissions.BROWSE, queryCreationContext.getUser()).getId()));
        }
    }

    public int getMinimumNumberOfExpectedArguments()
    {
        return 0;
    }

    public JiraDataType getDataType()
    {
        return JiraDataTypes.PROJECT;
    }
	
	public boolean isList()
	{
		return false;
	}
}
