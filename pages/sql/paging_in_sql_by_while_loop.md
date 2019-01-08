    def get_...(self, parameters):
        if "task_type_id" not in parameters:
            raise ValueError("Missing task_id in input parameters")
        if "env_id" not in parameters:
            raise ValueError("Missing env_id in input parameters")
                
        query = """
            SELECT *
            FROM
            (
            
                SELECT *, 
                row_number() OVER (ORDER BY SessionId) AS RowNumber
                FROM
                (
                    SELECT 
                    ...
            
                    FROM ...
                    JOIN ...
            
                    WHERE Table1.TaskId = @task_id AND Table2.EnvironmentId = @env_id 
                ) tableX 
            
            )tableY 
            WHERE tableY.RowNumber BETWEEN @start_row_num AND @end_row_num
            """
        
        try:
            sessions = []
            
            row_num_per_page = 1000
            start_row_num = 1
            end_row_num = row_num_per_page
            
            while True:
                
                # add the parameters for paging into the parameter list of
                # the query statement
                parameters["start_row_num"] = start_row_num
                parameters["end_row_num"] = end_row_num
                
                rows = self._process_query(query, parameters)
                
                # if we reach the end of all the pages of the query results
                if not rows:  
                    break

                for row in rows:
                    ...
                
                # prepare for the next loop
                start_row_num = end_row_num + 1
                end_row_num = start_row_num + row_num_per_page - 1
            
            LOGGER.info("The size of sessions array is: " + str(len(sessions)))
            
            return sessions
        
        except Exception as e:
            raise Exception("Failed to run query %s: %s", query, e)
