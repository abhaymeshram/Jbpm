# abhayJbpm

Quartz database Scripts : http://svn.terracotta.org/svn/quartz/tags/quartz-2.1.6/docs/dbTables/


https://devwithus.com/download-upload-files-with-spring-boot/






public class StartNewProcessWorkItemHandler implements org.kie.api.runtime.process.WorkItemHandler	{

	private String processId;
	private org.kie.api.runtime.process.ProcessWorkItemHandlerException.HandlingStrategy strategy;
	
	

	public StartNewProcessWorkItemHandler() {
		super();
		// TODO Auto-generated constructor stub
	}

	public StartNewProcessWorkItemHandler(String processId, String strategy) {
		super();
		this.processId = processId;
		this.strategy = org.kie.api.runtime.process.ProcessWorkItemHandlerException.HandlingStrategy.valueOf(strategy);
	}
	

	public StartNewProcessWorkItemHandler(String processId, org.kie.api.runtime.process.ProcessWorkItemHandlerException.HandlingStrategy strategy) {
		super();
		this.processId = processId;
		this.strategy = strategy;
	}

	public void executeWorkItem(org.kie.api.runtime.process.WorkItem workItem, org.kie.api.runtime.process.WorkItemManager manager) {
		if(processId != null && strategy != null) {
			throw new org.kie.api.runtime.process.ProcessWorkItemHandlerException(processId,strategy,new RuntimeException("On Purpose"));
		}
		manager.completeWorkItem(workItem.getId(), null);
	}

	public void abortWorkItem(org.kie.api.runtime.process.WorkItem workItem, org.kie.api.runtime.process.WorkItemManager manager) {
		// TODO Auto-generated method stub
		
	}
}


@Override
	public List<TaskSummary> getTasksAssignedAsPotentialOwner(String userId, List<String> groupIds, QueryFilter filter) {
	    Map<String, Object> params = new HashMap<String, Object>();
        params.put("userId", userId);        
        params.put("groupIds", mergeLists(groupIds, getCallbackUserRoles(userGroupCallback, userId)));
        applyQueryContext(params, filter);
        applyQueryFilter(params, filter);
        return (List<TaskSummary>) commandService.execute(new QueryNameCommand<List<TaskSummary>>	("TasksAssignedAsPotentialOwnerWithGroups",params));
	}
    public static List<String> getCallbackUserRoles(UserGroupCallback userGroupCallback, String userId) {
		List<String> roles = userGroupCallback != null ? userGroupCallback.getGroupsForUser(userId) : new ArrayList<>();
		if (roles == null || roles.isEmpty()) {
			roles = new ArrayList<>();
			roles.add("");
		}
		return roles;
	}
	@Override
	public List<TaskSummary> getTasksAssignedAsPotentialOwner(String userId, List<String> groupIds,	QueryFilter filter) {
		Map<String, Object> params = new HashMap<String, Object>();
		// params.put("groupIds", mergeLists(groupIds, getCallbackUserRoles(userGroupCallback, userId)));
		if (groupIds == null || groupIds.isEmpty()) {
			params.put("userId", userId);
			params.put("groupIds", getCallbackUserRoles(userGroupCallback, userId));
		} else 
			params.put("groupIds", filteredUserGroups(groupIds, getCallbackUserRoles(userGroupCallback, userId)));
		applyQueryContext(params, filter);
		applyQueryFilter(params, filter);
		return (List<TaskSummary>) commandService.execute(new QueryNameCommand<List<TaskSummary>>("TasksAssignedAsPotentialOwnerWithGroups", params));
	}
	private List<String> filteredUserGroups(List<String> groupIds, List<String> callbackUserRoles) {
		List<String> data = new ArrayList<String>();
		for (String groupId : groupIds) {
			if (callbackUserRoles.contains(groupId))
				data.add(groupId);
		}
		return data;
	}
