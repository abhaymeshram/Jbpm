# abhayJbpm

Quartz database Scripts : http://svn.terracotta.org/svn/quartz/tags/quartz-2.1.6/docs/dbTables/


public class StartNewProcessWorkItemHandler implements org.kie.api.runtime.process.WorkItemHandler{
	
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

